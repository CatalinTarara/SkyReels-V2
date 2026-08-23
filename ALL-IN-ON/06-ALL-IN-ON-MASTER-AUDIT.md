# ALL IN ON — MASTER AUDIT

Data: 2026-08-23 · Auditor: Claude Code (sesiune remote, read-only) · Cont: catalin.tarara.remote@gmail.com

## 1. Executive Summary

ALL IN ON este un sistem real, mai mare și mai avansat decât sugerau discuțiile anterioare: **63 de workflow-uri n8n active** (~170 total, 8 „departamente"), un plan de control în Notion cu 21 de unelte inventariate, un data layer Airtable proiectat la nivel de producție și un site live. Nu e un proiect de început — e un sistem în funcțiune.

Problema nu e că lipsesc componente. Sunt trei:

**Unu — sistemul stă pe un singur punct de eșec, fără plasă.** Tot ce contează rulează pe o VM Oracle Cloud. Nu există backup dovedit, nu există cod versionat, nu există export. Dacă VM-ul dispare — și Oracle Free Tier recuperează instanțe inactive — pierzi tot, ireversibil.

**Doi — există o breșă de securitate deschisă acum.** Un val.town **public** conține un secret de acces scris literal în codul sursă vizibil oricui, plus IP-ul VM-ului și forma comenzii SSH. Endpoint-ul protejat de acel secret controlează unde arată redirectul spre n8n. n8n, la rândul lui, rulează pe HTTP simplu, fără TLS. Lanțul e complet: sursă publică → secret → redirect controlat → pagină de login falsificabilă → credential store n8n (Telegram, Notion, Airtable, WhatsApp, OpenAI).

**Trei — schema depășește implementarea, iar evidența proprie e depășită.** Airtable are idempotență, retry, kill-switch per platformă și QA video pentru un motor de distribuție (Blotato) care, prin propria ta recunoaștere, „este un PLAN, nu o integrare". Iar Master Registry-ul afirmă „Make: ZERO scenarii" când sunt 4 — deci iei decizii pe un inventar care nu mai e adevărat.

Există și un risc deja materializat, nu potențial: Wingman a trimis e-mailuri reci autonome, fără aprobare, într-o perioadă (18–28.07) declarată neverificabilă.

**Recomandare:** oprește orice construcție nouă. Primele cinci acțiuni din §26 sunt toate read-only sau reversibile și reduc riscul major în câteva ore.

## 2. Inventar complet
Vezi `01-INVENTORY.md`. Rezumat: 1 VM Oracle (n8n) · 2 site-uri Netlify · 2 vals · 1 app Replit · 1 proiect Supabase (inactiv) · 3 baze Airtable · 4 scenarii Make (toate pauzate) · 2 baze Notion de control · 1 repo GitHub (fără legătură cu ALL IN ON) · ~15 servicii externe.

## 3. Arhitectură actuală
Vezi `02-ARCHITECTURE.md`. Notion → n8n → {Airtable, video, social, WhatsApp}, cu Telegram ca poartă de aprobare. Make, Replit, Supabase, Lovable sunt decuplate.

## 4. GitHub
`CatalinTarara`. Un singur repo accesibil: fork `SkyReels-V2`, fără legătură cu ALL IN ON. **Nu există cod ALL IN ON versionat.** Registry-ul propriu confirmă: `NEEDS CREDENTIAL`.

## 5. Local Projects
**UNKNOWN.** Auditul a rulat într-un container cloud; mașina ta nu e accesibilă de aici. Necesită o sesiune Claude Code locală.

## 6. n8n Infrastructure
Oracle Cloud VM `catalin-n8n-server`, Docker, n8n pe `:5678` **HTTP fără TLS**. Acces prin SSH (`oci_ed25519`) + `docker exec`. REST API **401**. IP dinamic urmărit de `oracle_gateway`. Reverse proxy: NOT FOUND. Backup: NOT FOUND.

## 7. Workflows
63 active / ~170 total / 8 departamente (din registry). 22 folosesc Telegram ca poartă de aprobare. `CIT-BRIDGE-002` (Notion→Telegram, 15 min) VERIFIED cu error handling și idempotență corecte — dovadă că știi să construiești bine când ai timp.

## 8. Databases
Airtable (3 baze; `Social Media Automation` = 6 tabele, schema matură) · PostgreSQL pe VM (introspecție blocată) · Supabase PG17 **INACTIVE** · Notion ca stare partajată.

## 9. APIs & Integrations
VERIFIED: Notion, Airtable, Netlify, Make, Replit, val.town, Supabase, Gmail, Drive, HeyGen.
BLOCKED/UNKNOWN: n8n REST (401), Blotato, JSON2Video, Metricool, Telegram, WhatsApp, Render, e-Matrix, Wingman.

## 10. AI Providers
OpenAI (prin Make + n8n), Claude Code, ChatGPT (plan de control, **workspace divergent**), HeyGen, JSON2Video, „ai-local-agent" (Make). Chei: `[PRESENT - SECRET REDACTED]` în credential store n8n; niciuna citită.

## 11. Social Automation
Publicare reală prin noduri native n8n (5 rețele). Blotato = plan neimplementat, cu 3 erori de publicare pe 17.08. Airtable pregătit pentru Blotato dar neconectat. Metricool: TikTok deconectat. LinkedIn: editare blocată pentru unelte. X/Reddit: handle-uri autogenerate.

## 12. CRM / Leads
**Cea mai mare gaură funcțională.** WhatsApp Cloud API e un pass-through de 3 noduri: fără clasificare, fără scoring, fără CRM, fără poartă de aprobare. Nu există CRM. Lead-urile intră și nu sunt procesate.

## 13. Payments / Invoicing
Stripe **NEACTIVAT** (nu poți încasa online). SOLO manual, CUI 54604995 — zonă interzisă, decizie fiscală luată. Render/Nuelink/Emergent: plăți eșuate. Gumroad: **NOT FOUND**.

## 14. Website / Funnel
freelancerhubpro.org live pe Netlify (deploy ready, forms enabled). MFA off. og:image lipsă. `gentle-mochi-f26534`: scop neidentificat. val.town `freelancerhubpro` în paralel.

## 15. Documentation
Notion = documentația vie și e de calitate. Referințe la fișiere locale (`TELEGRAM_CONSOLIDATION_AUDIT.md`, `03_WEBSITE_FIX_PREPARED.md`, `04_SOCIAL_PROFILES_READY_TO_PASTE.md`, `ALL IN ON/PORTFOLIO`) — **neverificabile de aici**, trăiesc pe mașina ta.

## 16. Security
Vezi C1–C2 din `04-RISK-REGISTER.md`. Nicio valoare de secret nu a fost citită, afișată sau stocată. Toate marcate `[PRESENT - SECRET REDACTED]`.
Suplimentar: Netlify fără MFA · Make cu 2FA ON (bine) · material `DO_NOT_PUBLISH` a fost public în bio-ul X.

## 17. Backups
**NOT FOUND. Zero, pe toate nivelurile:** workflow-uri n8n, PostgreSQL, credențiale, configurații, volume Docker. Acesta e riscul cu cea mai mare pierdere așteptată din tot auditul.

## 18. Critical Problems
C1 val public cu secret · C2 n8n fără TLS · C3 zero backup · C4 Telegram 3-triggere · C5 Wingman outreach neverificat.

## 19. High Priority Problems
H1 Stripe neactivat · H2 lanț plăți eșuate · H3 Blotato plan vs. realitate · H4 API 401 · H5 Notion divergent · H6 Netlify fără MFA · H7 portofoliu Upwork contradictoriu · H8 Metricool TikTok.

## 20. Medium / Low Priority
M1–M8, L1–L4 în `04-RISK-REGISTER.md`.

## 21. Missing Components
CRM · lead scoring · backup & restore · monitorizare/alerting pe workflow-uri · versionare cod · CI/CD · TLS · staging (totul e producție) · error budget · plan de recuperare.

## 22. Dependencies
```
Oracle VM ──► n8n ──► TOT
n8n ──► Airtable, Telegram, WhatsApp, social, JSON2Video, HeyGen
Notion ──► n8n (CIT-BRIDGE-002) ──► Telegram
oracle_gateway ──► accesul tău la n8n (IP dinamic)
Netlify ──► freelancerhubpro.org ──► funnel ──► (Stripe LIPSĂ)
Decuplate: Make · Replit · Supabase · Lovable · GitHub
```

## 23. Single Points of Failure
1. **Oracle VM** — fără backup, fără redundanță. SPOF absolut.
2. **`oracle_gateway`** — dacă pică val-ul sau IP-ul se schimbă fără update, îți pierzi accesul.
3. **Un singur bot Telegram** pentru 22 de workflow-uri, cu 3 triggere care se calcă.
4. **Un singur card** pentru toate abonamentele — deja eșuează pe 4 servicii.
5. **Tu** — fiecare `Next Action` din registry cere decizia sau mâna ta.

## 24. Recommended Target Architecture
Vezi `02-ARCHITECTURE.md` §TARGET STATE. Principiul: **sursa de adevăr se mută de pe VM în Git**, iar n8n devine reconstruibil, nu pierdut.

## 25. Execution Backlog
Vezi `05-BACKLOG.md` — tabel prioritizat P0→P3.

## 26. Next Safe Actions
1. Privatizează `oracle_gateway` + rotește secretul (minute, risc maxim eliminat)
2. Exportă toate workflow-urile n8n într-un repo privat (citire pură)
3. Verifică folderul Sent 18–28.07 pentru Wingman (citire pură)
4. Test `/status` pe Telegram (citire pură)
5. Decide: actualizezi cardul sau anulezi abonamentele nefolosite

---

# AUDIT STATUS

**AUDIT STATUS:** COMPLET pentru sursele accesibile · PARȚIAL global (local + n8n inaccesibile)

**SOURCES CHECKED (14):** filesystem container · GitHub · Notion (2 baze) · Airtable (3 baze) · Make · Netlify · Replit · val.town · Supabase · Lovable · Gmail · Google Drive · conectori MCP · registry propriu

**SOURCES BLOCKED (10):** filesystem local · n8n server · Oracle VM · Render · Blotato · JSON2Video · Metricool · Telegram · WhatsApp Cloud API · Tropic (OAuth)

**PROJECTS FOUND:** 12 active (n8n stack, freelancerhubpro.org, 2 vals, N8n Proxy, 3 baze Airtable, 4 scenarii Make, 2 baze Notion, Supabase inactiv, 2 site-uri Netlify)

**CRITICAL ISSUES:** 5 (C1–C5)

**P1 ISSUES:** 8 (H1–H8)

**MISSING ACCESS:** n8n API key / SSH · sesiune locală · dashboard Render · API Blotato · repo-uri GitHub private

**NEXT SAFE ACTIONS:** cele 5 de la §26 — toate read-only sau reversibile

**MODIFICĂRI OPERAȚIONALE EFECTUATE: ZERO.** Nimic instalat, șters, mutat, publicat sau deployat. Niciun server atins. Nicio valoare de secret citită sau afișată.
