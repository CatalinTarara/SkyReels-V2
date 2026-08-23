# 05 — EXECUTION BACKLOG

Ordinea contează: fiecare P0 e o condiție pentru restul. **Nimic nu a fost executat.**

| PRIORITY | COMPONENT | PROBLEM | IMPACT | DEPENDENCY | RECOMMENDED ACTION |
|---|---|---|---|---|---|
| P0 | val.town `oracle_gateway` | Val public cu secret hardcodat + IP VM | Compromitere n8n completă | — | Privatizează val-ul, mută secretul în env, rotește-l, șterge `/ssh` |
| P0 | Oracle VM / n8n | :5678 pe HTTP simplu, expus public | Furt de sesiune/credențiale | C1 | Reverse proxy + TLS; închide 5678 public |
| P0 | n8n workflows | Zero backup pentru ~170 workflow-uri | Pierdere totală ireversibilă | acces SSH | `n8n export:workflow --all` → repo privat; apoi cron zilnic |
| P0 | Telegram | 3 triggere pe 1 bot | Poarta de aprobare posibil moartă | acces n8n | Test `/status`; migrare per `TELEGRAM_CONSOLIDATION_AUDIT.md` |
| P0 | Wingman | Outreach autonom neverificat 18–28.07 | Expunere GDPR deja produsă | acces email | Audit manual folder Sent; NU realimenta credite |
| P1 | Plăți | Render/Nuelink/Emergent eșuate | Suspendare servicii în cascadă | decizie CEO | Actualizează card SAU anulează ce nu folosești |
| P1 | Stripe | Cont neactivat | Nu poți încasa online | date business | Completează profilul sau acceptă SOLO manual |
| P1 | n8n API | 401, cheie lipsă | Blochează backup automat | acces n8n | Emite cheie API din Settings |
| P1 | Distribuție | Blotato = plan, nu integrare; 3 erori | Două arhitecturi incomplete | decizie CEO | Alege A (n8n nativ) sau B (Blotato). Nu ambele |
| P1 | Notion | 2 workspace-uri divergente | Planul de control e rupt în două | ChatGPT | Un singur workspace: `da3c8305…` |
| P1 | Netlify | MFA off + og:image lipsă | Risc cont + share-uri goale | — | Activează MFA; adaugă og-image 1200x630 |
| P1 | Upwork | Portofoliu = mostre CV | Prioritate #1 se contrazice | decizie CEO | Arhivează CV items, adaugă P01–P04, corectează nivelul de engleză |
| P2 | Make.com | 2 scenarii invalide, org pauzată | Automatizări moarte | — | Repară sau șterge; actualizează registry-ul |
| P2 | Master Registry | Afirmă „Make: ZERO scenarii" (fals) | Deciziile pe date greșite | — | Corectează la 4 scenarii |
| P2 | Supabase | INACTIVE, rol nedefinit | Poate fi șters de platformă | — | Definește rolul sau șterge deliberat |
| P2 | Replit N8n Proxy | Monorepo fără rol clar | Efort duplicat | — | Documentează scopul sau arhivează |
| P2 | Metricool | TikTok deconectat | Fără măsurare | — | Reconectează după ce există distribuție |
| P2 | X / Reddit | Handle-uri autogenerate | Brand inconsistent | — | Schimbă handle X; decide cont nou Reddit |
| P3 | Naming | Site-uri/baze cu nume generate | Igienă | — | Redenumire |
| P3 | Google Drive | Fișiere neclasificate | Igienă | — | Clasifică sau șterge |

## NEXT SAFE ACTIONS (fără risc, executabile imediat)
1. **Privatizează `oracle_gateway`** — un singur toggle în val.town. Cea mai mare reducere de risc per efort din toată lista.
2. **Exportă workflow-urile n8n** — citire pură, elimină riscul de pierdere totală.
3. **Verifică folderul Sent** pentru 18–28.07 — citire pură, cuantifică expunerea Wingman.
4. **Test `/status` pe Telegram** — citire pură, confirmă dacă poarta de aprobare trăiește.
5. **Decide plățile** — actualizare card sau anulare deliberată.

Punctele 1–4 nu strică nimic dacă sunt greșite. Punctul 5 e o decizie de business, nu tehnică.
