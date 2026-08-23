# ALL IN ON — Inventar central (doar citire)

**Data:** 2026-08-23
**Cont:** catalin.tarara.remote@gmail.com
**Mediu de execuție:** container remote Claude Code (efemer), repo `CatalinTarara/SkyReels-V2`, branch `claude/all-in-on-inventory-audit-rugq4i`
**Regim:** strict read-only. Nu s-a instalat nimic, nu s-au generat chei SSH, nu s-a cerut OAuth nou, nu s-a modificat niciun serviciu.

---

## 0. Clarificare importantă despre "local"

Acest raport **NU** a fost produs pe laptopul tău. Rulează într-un container cloud izolat.
Concret: `~/ALL-IN-ON`, `/home/catalintarara`, `Documente`, `Descărcări`, `Desktop` **nu există aici** și nu pot fi inspectate din această sesiune.
Ce s-a verificat efectiv în container:

| Verificare | Rezultat |
|---|---|
| Utilizator / HOME | `root` / `/root` |
| Repo-uri git pe disc | doar `/home/user/SkyReels-V2` (+ `/opt/rbenv`, `/opt/nvm` — infrastructură, nu proiecte) |
| Fișiere/foldere `*all-in-on*` sau `*n8n*` pe disc | **niciunul** |
| Chei SSH (`~/.ssh`) | gol — Git merge prin HTTPS + token efemer, nu prin SSH |
| Spațiu disc | 252 GB total, ~30 GB liberi în alocare |

**Concluzie:** inventarul proiectelor locale de pe mașina ta trebuie făcut într-o sesiune Claude Code rulată local. Aici avem acces doar la GitHub (scop limitat) + conectorii cloud.

---

## 1. GitHub

| Câmp | Valoare |
|---|---|
| Utilizator autentificat | `CatalinTarara` (Catalin Irinel Tarara, România) |
| Cont creat | 2025-04-19 |
| Repo-uri publice | 1 |
| Repo-uri accesibile în sesiune | `CatalinTarara/SkyReels-V2` (public, **fork**, push permis) |
| Auditabil | Da (conținut cod) |
| Modificabil | Da, dar doar acest repo și doar pe branch-ul desemnat |
| Credențiale | token efemer al sesiunii; nu există SSH |

`list_repos` returnează **un singur** repository accesibil. Nu există în acest scope niciun repo numit ALL-IN-ON. Dacă ai repo-uri private sub alt cont/organizație, ele nu sunt vizibile fără extinderea accesului din setările GitHub ale Claude.

---

## 2. Proiecte locale

Neaplicabil în acest mediu (vezi secțiunea 0). Status: **neinventariat**, nu "inexistent".

---

## 3. n8n — server & workflow-uri

**Nu există conector n8n** instalat la nivel de cont și nici configurație n8n în container. Deci n8n **nu poate fi auditat direct** din această sesiune.

Dovezi indirecte că ai un n8n activ (din Gmail, doar metadate/snippet-uri):

| Semnal | Detaliu |
|---|---|
| n8n Cloud | e-mail 2026-08-18 de la `noreply@transactional.n8n.io` — "Your magic sign in link for n8n cloud" → **există un workspace n8n Cloud pe acest e-mail** |
| Replit | app `N8n Proxy` (`replit.com/@catalintararare/N8n-Proxy`), actualizat 2026-04-18 |
| Notion | pagini `CIT-BRIDGE-002 — Notion → n8n → Telegram trigger`, `CIT-BRIDGE-002-TEST` |
| Skool | comunități n8n (digest-uri, evenimente) |

Credențiale necesare pentru audit real: acces la workspace-ul n8n Cloud (URL instanță + login) sau un API key n8n. **Nu a fost cerut și nu a fost folosit.**

---

## 4. Conectori / MCP disponibili (acces real, la nivel de cont)

Conectate și active în sesiune: Google Drive, Gmail, Google Calendar, Notion, Airtable, Supabase, Make, Netlify, Lovable, Replit, val.town, Linear, Asana, ClickUp, monday.com, Atlassian Rovo, HubSpot, Close, Canva, Figma, Wix, Zapier, Semrush, Sentry, Twilio, PayPal, Airwallex, Zoom, SlidesGPT, Manufact, Upwork, Adobe Experience Manager, Legal Data Hunter, CMS Coverage, MindMap, PDF Viewer.

Neconectate / indisponibile: Slack, Microsoft 365, Stripe, Notion-adiacente (Guru), Coralogix, Amplitude, Similarweb, WordPress.com, Zoho Projects, Vanta, ș.a.
**Tropic** apare instalat dar necesită autorizare OAuth — nu poate fi rulat dintr-o sesiune non-interactivă.

---

## 5. Active descoperite pe platforme (inventar concret)

| Platformă | Nume | Tip | Status | Conținut | Auditabil | Modificabil |
|---|---|---|---|---|---|---|
| GitHub | CatalinTarara/SkyReels-V2 | repo public (fork) | activ | cod SkyReels-V2 | da | da (branch desemnat) |
| Supabase | `Access Shared Link` (`qirzdpexofoytgqqosye`, eu-west-2, PG 17) | proiect DB | **INACTIVE (pauzat)** | necunoscut până la repornire | parțial | da (nerecomandat acum) |
| Netlify | `effulgent-cupcake-906882` → **freelancerhubpro.org** | site live | deploy `ready` | site FreelancerHub Pro, forms enabled | da | da |
| Netlify | `gentle-mochi-f26534` | site | deploy `ready` | fără domeniu custom | da | da |
| Replit | `N8n Proxy` | app | ultima modificare 2026-04-18 | proxy către n8n | da | da |
| val.town | `catalinirineltarara/oracle_gateway` | val public HTTP | activ | gateway | da | da |
| val.town | `catalinirineltarara/freelancerhubpro` | val public HTTP | activ | backend/pagini FreelancerHub | da | da |
| Airtable | `Social Media Automation` (favorit), `Social Media`, `Untitled Base` | baze | active | automatizări social | da | da |
| Make | org `My Organization` (eu2, plan **Free**, `isPaused: true`), team `Administrator Ai ChatGBT` | automatizări | **pauzat** | max 2 scenarii, 1000 op/lună | da | da |
| Lovable | workspace `Catalin's Lovable` (free) | workspace | activ | **0 proiecte** | da | da |
| Notion | workspace personal | documentație | activ | `Projects` (DB), `CIT-BRIDGE-002 …`, `Template Pachet Servicii AI`, `Tabel platforme marketing`, `Curățare portofoliu Upwork` | da | da |
| Google Drive | `nano_banana_ad_creative_generator.json`, spreadsheet fără titlu + materiale partajate | fișiere | active | workflow JSON + docs | da | da |

---

## 6. Semnale de risc observate (fără acțiune)

1. **Render — plată eșuată.** Trei e-mailuri (17, 19, 21 aug 2026): "Invalid payment info on Render", sold neplătit. Există deci și un serviciu **Render** în stack-ul tău, neinventariat mai sus pentru că nu are conector. Risc: suspendarea serviciilor găzduite acolo.
2. **Supabase `Access Shared Link` este INACTIVE** — proiectele pauzate pot fi șterse după perioade lungi de inactivitate.
3. **Make este pe pauză**, plan Free (2 scenarii, 1000 operațiuni).
4. **Fără SSH și fără backup local vizibil** — tot ce nu e în cloud sau în Git nu e inventariat nicăieri.

---

## 7. Ce lipsește pentru harta completă

| Sursă | Ce trebuie |
|---|---|
| Proiecte locale | rulare Claude Code **local**, pe mașina ta |
| n8n | URL instanță n8n Cloud + acces (sau API key) |
| Render | acces cont Render (nu există conector — verificare manuală în dashboard) |
| GitHub privat / alte org-uri | extinderea accesului repo din setările Claude pentru GitHub |
| Tropic | autorizare OAuth dintr-o sesiune interactivă |

---

## 8. Recomandare de pas următor (nu executat)

Ordinea logică, tot fără modificări: (1) rezolvă plata Render înainte să pierzi servicii; (2) deschide n8n Cloud și exportă lista de workflow-uri; (3) inventariază local; (4) abia apoi decidem consolidarea într-un singur repo ALL-IN-ON.

*Raport generat exclusiv prin operațiuni de citire. Niciun serviciu nu a fost modificat, pornit, oprit sau șters.*
