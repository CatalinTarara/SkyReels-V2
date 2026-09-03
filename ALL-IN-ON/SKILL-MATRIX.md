# SKILL MATRIX — cine ce poate face

Data: 2026-09-03 · Verificat prin apeluri reale, nu din memorie.

De ce există acest fișier: auditul din 3 septembrie a arătat că cei trei agenți (Claude Code, ChatGPT, Perplexity) au auditat trei proiecte diferite, fără suprapunere. Cauza: nimeni nu știa ce poate face celălalt. Fiecare cerere care a eșuat în ultimele săptămâni a eșuat pe o limită care nu era scrisă nicăieri.

Legendă: **DA** = verificat prin apel reușit · **NU** = verificat că nu se poate · **CU CHEIE** = posibil dacă primesc credențiala · **NEVERIFICAT** = nu am testat

---

## 1. Claude Code (sesiune remote, container efemer)

### Poate

| Capabilitate | Stare | Notă |
|---|---|---|
| Citit/scris fișiere în repo `CatalinTarara/SkyReels-V2` | DA | Singurul repo în scope |
| Commit + push + PR | DA | Doar pe branch-ul desemnat |
| Bash, rulare cod, teste | DA | În container, efemer |
| HTTP/HTTPS outbound | DA | Prin proxy. `http://example.com` → 200 |
| Notion: citit, creat pagini/baze, query | DA | Verificat 03.09 |
| val.town: listat, citit, editat cod, deploy | DA | Verificat 03.09 — am șters `/ssh` |
| Gmail `catalin.tarara.remote@`: căutare, etichetare, draft | DA | |
| Creat/șters routines (trigger-e programate) | DA | Verificat 03.09 |
| Netlify, GitHub, Make, Airtable, Stripe, Figma etc. | NEVERIFICAT | Conectori prezenți dar instabili în sesiune |

### Nu poate

| Ce | De ce | Consecință |
|---|---|---|
| Vedea browserul tău / taburile deschise | Nu există tool | Trimite URL sau screenshot |
| Extensie Chrome | Nu am | — |
| Vorbi cu ChatGPT / Perplexity / Wingman / e-matrix | Produse în spatele login-ului, fără API configurat | Copiezi tu briefurile |
| Trimite mesaj pe telefonul tău | Fără conector Telegram/SMS/push | — |
| SSH la Oracle VM | Fără cheie, `~/.ssh` gol | **CU CHEIE** — dar cheia e pe mașina ta |
| Accesa n8n | Fără API key. `92.4.162.138:5678` → timeout | **CU CHEIE** — vezi mai jos |
| Posta pe Instagram | Fără token Meta Graph API | **CU CHEIE** |
| DM automat Instagram | Necesită app aprobat de Meta (review) | Blocat de Meta, nu de mine |
| Face val.town privat | Cont free tier — limita de proiecte private atinsă | Necesită upgrade |
| Citi filtrele Gmail | API-ul nu expune endpoint-ul | Nu pot verifica dacă filtrele există |
| Trimite propuneri pe Freelancer/Upwork | Regulă proprie + fără sesiune autentificată | Tu trimiți |
| Vedea disc local, alte repo-uri, sesiuni terminal | Container izolat | — |
| Ține minte între sesiuni | Context efemer | Tot ce contează → repo |

### Ce deblochează accesul

| Îmi dai | Deblochează |
|---|---|
| **n8n API key + URL curent** | Audit complet n8n, export ~170 workflows, backup automat, workflow-uri noi |
| **Token Meta Graph API** | Postare automată Instagram |
| **IP curent Oracle VM** | Verificare dacă VM-ul trăiește |
| **Upgrade val.town** | Privatizare `oracle_gateway` |

---

## 2. ChatGPT (proiect ALL IN ON)

| Capabilitate | Stare |
|---|---|
| Strategie, decizii de business, arhitectură | DA — rolul principal |
| Memorie lungă între sesiuni | DA — știe istoric pe care Claude Code nu-l are |
| Vizibilitate active: profiluri, video, template-uri | DA — a raportat JSON2Video, HeyGen, YouTube, Analytics |
| Vizibilitate repo git | NU |
| Vizibilitate Notion live | NU |
| Execuție tehnică, cod, deploy | NU |
| Verificare independentă a ce i se spune | NU — DOVEDIT înseamnă „mi s-a comunicat" |

**Modul de eroare:** acceptă declarația ca dovadă. Pe 03.09 marca `JOB INBOX` și `CLAUDE TASKS` ca livrate; primul era arhivat de tine cu „NU FOLOSI", al doilea nu a existat niciodată.

---

## 3. Perplexity

| Capabilitate | Stare |
|---|---|
| Căutare web, verificare surse externe | DA |
| Citit fișiere încărcate în Space | DA |
| Vizibilitate repo git | NU — fișierele pe care le citează nu există în repo |
| Vizibilitate Notion, n8n, val.town | NU |
| Execuție | NU |

**Modul de eroare:** raportează dimensiuni exacte în bytes ale fișierelor din Space-ul propriu, ceea ce sună a verificare tehnică pe proiect. Nu e — sunt fișiere izolate.

---

## 4. n8n (Oracle VM)

| Capabilitate | Stare |
|---|---|
| ~170 workflows, 63 active | DECLARAT — neverificat de nimeni din august |
| Răspunde la `92.4.162.138:5678` | **NU** — timeout 20s, verificat 03.09 |
| Backup | **NU EXISTĂ** — P0 deschis din 23.08 |
| API key | Nu a fost emisă |

Cel mai probabil: VM repornit, IP nou, gateway-ul servește IP-ul din iulie. Sau VM oprit.

---

## 5. Cătălin — ce doar tu poți face

| Acțiune | De ce doar tu |
|---|---|
| Emis cheie API n8n | Cere login n8n |
| Confirmat IP curent Oracle | Cere consola Oracle Cloud |
| Trimis propuneri Freelancer/Upwork | 2FA + regulă de aprobare |
| Token Meta Graph API | Cere Business Manager |
| MFA pe Netlify | Cere telefonul tău |
| Test `/status` Telegram | Fără conector la mine |
| Verificat folderul Sent 18–28 iulie (Wingman/GDPR) | Deschis din 23.08, neatins |
| Aprobat orice acțiune publică sau ireversibilă | Regula sistemului |
| Copiat briefuri între agenți | Singura punte reală între noi |

---

## 6. Reguli de securitate — permanente

- Niciodată nu afișez chei, parole, tokenuri, cookies, secrete. Marcaj: `[PREZENT — REDACTAT]`
- Niciun secret în Notion sau în repo
- Nicio propunere trimisă fără `WAITING_APPROVAL` + aprobare explicită
- Fără ocolire CAPTCHA/2FA
- Fără bulk spam sau mesaje identice
- SOLO (falcon.solo.ro, CUI 54604995): **zonă interzisă** — nu se închide abonamentul, nu se migrează
- Wingman: fără realimentare credite până la auditul manual al folderului Sent

---

## Cum se folosește

Înainte de a cere ceva unui agent, verifică în tabelul lui dacă poate. Dacă scrie **CU CHEIE**, dă-i cheia. Dacă scrie **NU**, cere altcuiva sau fă-o tu.

Actualizează acest fișier când o limită se schimbă. E singurul loc unde scrie cine ce poate.
