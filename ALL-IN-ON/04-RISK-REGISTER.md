# 04 — RISK REGISTER

Format per problemă: PROBLEMĂ · IMPACT · DOVADĂ · LOCAȚIE · DEPENDENȚE · RISC · RECOMANDARE · PRIORITATE
**Nimic din ce urmează nu a fost reparat.** Doar constatat.

---

## CRITICAL

### C1 — Val public expune calea către n8n, cu secret hardcodat în cod public
**PROBLEMĂ:** `oracle_gateway` este un val **public** (`privacy: public`, `httpPrivacy: public`). Codul sursă e vizibil oricui și conține (a) un secret de actualizare scris literal în sursă și (b) un IP public de fallback al VM-ului Oracle. Endpoint-ul `/update` acceptă un IP nou pe baza acelui secret; `/ssh` publică forma comenzii SSH și numele cheii; `/n8n` redirectează la `http://<IP>:5678`.
**IMPACT:** Oricine găsește URL-ul poate citi secretul din sursa publică și poate rescrie pointer-ul de IP — adică poate redirecționa `/n8n` și orice script care consumă `/ip` către un server controlat de el. Combinat cu faptul că n8n rulează pe HTTP simplu, aceasta e o cale directă de furt de credențiale prin pagina de login falsificată.
**DOVADĂ:** `mcp__valtown__read_file catalinirineltarara/oracle_gateway main.tsx` — constante `UPDATE_SECRET` și `DEFAULT_IP` la nivel de modul. `list_vals` → `privacy: "public"`.
**LOCAȚIE:** val.town `catalinirineltarara/oracle_gateway`
**DEPENDENȚE:** Oracle VM · n8n · orice script care citește `/ip`
**RISC:** Compromitere completă a instanței n8n și a tuturor credențialelor din credential store (Telegram, Notion, Airtable, WhatsApp, OpenAI).
**RECOMANDARE:** Trece val-ul pe `privacy: private` + `httpPrivacy: restricted`; mută secretul în env var (val.town suportă); rotește secretul; elimină `/ssh` (nu publica numele cheii). Apoi restricționează portul 5678 la nivel de Security List Oracle.
**PRIORITATE: P0**
**STATUS 2026-09-03:** Parțial rezolvat. Secretul nu mai e în sursă (`Deno.env.get`, din 23.08). `/ssh` eliminat → 410, verificat live. Rămân: cod public (free tier blochează `private`), `DEFAULT_IP` în clar, `/n8n` redirect pe HTTP. Prioritate coborâtă la P1.

### C2 — n8n expus pe HTTP simplu, fără TLS, direct pe internet
**PROBLEMĂ:** UI-ul n8n e servit pe `http://<IP>:5678`, fără reverse proxy și fără certificat.
**IMPACT:** Login-ul n8n (și cookie-ul de sesiune) circulă în clar. Orice intermediar de rețea le poate citi.
**DOVADĂ:** `oracle_gateway/main.tsx` — `Response.redirect("http://${ip}:5678")`. Registry Notion: `Access Method: SSH + docker exec`. Niciun reverse proxy găsit.
**LOCAȚIE:** Oracle Cloud VM
**DEPENDENȚE:** toate cele 63 de workflow-uri active
**RISC:** Preluarea completă a n8n.
**RECOMANDARE:** Caddy sau nginx cu Let's Encrypt în fața n8n; 5678 accesibil doar din localhost; `N8N_SECURE_COOKIE=true`.
**PRIORITATE: P0**

### C3 — Zero backup dovedit pentru ~170 workflow-uri
**PROBLEMĂ:** Nu există nicio dovadă de backup, export sau versionare pentru workflow-urile n8n. GitHub nu conține cod ALL IN ON.
**IMPACT:** Pierderea VM-ului = pierderea întregii logici de business. Ireversibil.
**DOVADĂ:** `list_repos` → 1 repo (fork SkyReels). Registry: GitHub `NEEDS CREDENTIAL`, „versionarea rămâne locală". Niciun fișier de export găsit.
**LOCAȚIE:** Oracle Cloud VM (single point of failure)
**DEPENDENȚE:** TOT sistemul
**RISC:** Pierdere totală, nerecuperabilă. Oracle Free Tier recuperează instanțele inactive.
**RECOMANDARE:** Înainte de orice altă lucrare: `n8n export:workflow --all` + export credentials **fără valori**, într-un repo privat. Apoi cron zilnic.
**PRIORITATE: P0**

### C4 — Telegram: 3 triggere active pe un bot care acceptă un singur webhook
**PROBLEMĂ:** 3 noduri `telegramTrigger` active pe același bot. Telegram permite UN webhook per bot.
**IMPACT:** 2 din 3 fluxuri nu primesc nimic. Poarta de aprobare umană pentru 22 de workflow-uri poate fi una dintre cele moarte — ceea ce înseamnă acțiuni care așteaptă o aprobare ce nu ajunge niciodată, sau se execută fără ea.
**DOVADĂ:** Master Registry, rând Telegram, Status VERIFIED cu acest blocker explicit.
**LOCAȚIE:** n8n · @CatalinBot + @n8n_mastercontrol_bot
**RISC:** Acțiuni comerciale neaprobate; comenzi pierdute.
**RECOMANDARE:** Test `/status`. Dacă nu răspunde, execută migrarea din `TELEGRAM_CONSOLIDATION_AUDIT.md` (9 pași — document care există la tine local).
**PRIORITATE: P0**

### C5 — Wingman: outreach autonom neverificat, 18–28.07
**PROBLEMĂ:** 3 task-uri de outreach autonom trimiteau e-mailuri reci **fără aprobare**. Pauzat 14.08. Perioada 18–28.07 e declarată NEVERIFICABILĂ din datele proprii.
**IMPACT:** E-mailuri comerciale nesolicitate trimise în numele tău, cu conținut necunoscut, către destinatari necunoscuți. Expunere GDPR și de reputație.
**DOVADĂ:** Master Registry, rând Wingman, Status BLOCKED.
**DEPENDENȚE:** conturile de e-mail folosite
**RISC:** Reclamații GDPR, blacklist de domeniu, daune de brand — deja produse, nu potențiale.
**RECOMANDARE:** Verifică manual folderul **Sent** al conturilor pentru 18–28.07 și cuantifică. NU realimenta credite până atunci. Nu am făcut această verificare — necesită decizia ta.
**PRIORITATE: P0**
**STATUS 2026-09-03:** Auditat. ~61 thread-uri trimise 18–28.07 către firme reale (retail RO, avocatură US/UK, agenții AI), cu follow-up-uri, inclusiv destinatari nominali din UE. Cel puțin un template afirmă „12000+ production AI workflows" — fals. Riscul e confirmat, nu potențial. Rămân: verificarea răspunsurilor primite și corectarea afirmației față de cei care au răspuns.

---

## HIGH

### H1 — Stripe neactivat: nu poți încasa
**DOVADĂ:** Gmail — „Complete your business profile to unlock live payment processing"; „[Action required] Review your account representative".
**IMPACT:** Funnel-ul freelancerhubpro.org nu are cale de plată funcțională. Facturarea merge doar manual prin SOLO.
**RECOMANDARE:** Completează profilul, sau decide explicit că SOLO manual rămâne singura cale. **PRIORITATE: P1**

### H2 — Lanț de plăți eșuate pe 4 servicii
Render (3 notificări, sold neplătit) · Nuelink $144 (3 eșecuri) · Emergent Labs 94.50 lei · toate din același card.
**IMPACT:** Suspendare în cascadă. Render găzduiește ceva neinventariat.
**RECOMANDARE:** Actualizează cardul SAU anulează deliberat abonamentele nefolosite. Nuelink și Blotato par să acopere aceeași funcție — plătești de două ori. **PRIORITATE: P1**

### H3 — Blotato: 3 erori de publicare, integrare inexistentă
**DOVADĂ:** Gmail 2026-08-17 — erori LinkedIn, YouTube, Instagram. Registry: „PLAN, nu integrare".
**IMPACT:** Airtable are schema completă pentru Blotato (Account ID, targetType, idempotency) dar publicarea reală merge prin noduri n8n. Două arhitecturi paralele, niciuna completă.
**RECOMANDARE:** DECIZIE CEO: (A) n8n nativ rămâne motorul — zero cost, funcționează; (B) construiește Blotato complet. **Nu ambele.** **PRIORITATE: P1**

### H4 — n8n REST API returnează 401
Cheia lipsește din `user_api_keys`. CLI funcționează ca alternativă, dar orice automatizare care *modifică* workflow-uri e blocată — inclusiv backup-ul automat de la C3. **PRIORITATE: P1**

### H5 — Notion: două workspace-uri divergente
ChatGPT scrie în `27d4734a…`, tu citești `10dee1df…`. Fetch încrucișat = 404. Cele două jumătăți ale planului de control nu se văd. **PRIORITATE: P1**

### H6 — Netlify fără MFA
`mfa_enabled=false` pe contul care deployează freelancerhubpro.org. Plus og:image lipsă pe live (fiecare share arată gol). **PRIORITATE: P1**

### H7 — Upwork/Freelancer: portofoliu contradictoriu
Upwork conține EXCLUSIV mostre de redactare CV; zero automatizare. Engleză declarată „De bază" în timp ce overview-ul e în engleză fluentă. Prioritate comercială #1, prezentare care contrazice serviciul. **PRIORITATE: P1**

### H8 — Metricool: TikTok deconectat
Acces revocat. Fără măsurare pe canalul respectiv. **PRIORITATE: P2** (nu există încă trafic real de măsurat)

---

## MEDIUM

- **M1** — Make: 2 din 4 scenarii au `isinvalid: true`; org pe pauză, plan Free. Registry-ul afirmă „ZERO scenarii" — **inventar greșit în propria ta evidență**.
- **M2** — „🏠 Prospector Imobiliare Prahova" e off-brand față de CiT Automation Architect. La fel „Fashionista Boutique", „Elite Fitness" pe Freelancer.com.
- **M3** — Supabase INACTIVE, rol nedefinit. Proiectele pauzate pot fi șterse.
- **M4** — Replit „N8n Proxy": monorepo TS serios (Express 5, Drizzle, Zod, Orval), fără rol clar în arhitectură. Duplică sau înlocuiește ceva?
- **M5** — Netlify `gentle-mochi-f26534`: site fără scop identificat.
- **M6** — X/Twitter: handle autogenerat `@IrinelTara14498`; bio conținea material `DO_NOT_PUBLISH` public.
- **M7** — Reddit: username autogenerat, nerenominabil.
- **M8** — Lovable: workspace gol, cont nefolosit.

## LOW
- **L1** — Netlify `effulgent-cupcake-906882`: nume generat, nu de brand.
- **L2** — Airtable „Untitled Base" nedenumită.
- **L3** — Google Drive: „Foaie de calcul fără titlu", `nano_banana_ad_creative_generator.json` neclasificat.
- **L4** — Pinterest/Threads: doar aplicare manuală de branding.
