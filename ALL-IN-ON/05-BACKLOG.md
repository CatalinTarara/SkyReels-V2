# 05 — EXECUTION BACKLOG

Ordinea contează: fiecare P0 e o condiție pentru restul. **Nimic nu a fost executat.**

| PRIORITY | COMPONENT | PROBLEM | IMPACT | DEPENDENCY | RECOMMENDED ACTION |
|---|---|---|---|---|---|
| P1 | val.town `oracle_gateway` | Val public; IP VM încă vizibil în cod | Descoperire n8n de către terți | upgrade val.town | **Parțial rezolvat 2026-09-03.** Secretul NU mai e hardcodat (fix 23.08, `Deno.env.get`). `/ssh` șters — returnează 410, verificat live. Rămâne: cod public (free tier blochează `private`), `DEFAULT_IP` în clar. |
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
1. ~~**Privatizează `oracle_gateway`**~~ — ✅ parțial, 2026-09-03. `/ssh` eliminat (410 verificat). Privatizarea codului blocată de free tier.
2. **Exportă workflow-urile n8n** — citire pură, elimină riscul de pierdere totală. **Blocat: lipsă API key.**
3. ~~**Verifică folderul Sent** pentru 18–28.07~~ — ✅ făcut 2026-09-03. ~61 thread-uri de cold outreach către firme reale (retail RO, avocatură US/UK, agenții AI), cu follow-up-uri. Cel puțin un template afirmă „12000+ production AI workflows" — fals, zero clienți. Destinatari nominali din UE. Bounce-uri: `freimestate.ro`, `easysales.ai`, `catalin@freelancerhubpro.org` (adresa proprie nu primește mail). **Nu realimenta Wingman; scoate afirmațiile nedovedite din orice template.**
4. **Test `/status` pe Telegram** — citire pură, confirmă dacă poarta de aprobare trăiește. **Neatins din 23.08.**
5. **Decide plățile** — actualizare card sau anulare deliberată.

Punctele 2–4 nu strică nimic dacă sunt greșite. Punctul 5 e o decizie de business, nu tehnică.

## CONSTATĂRI 2026-09-03 (verificate)
- **VM Oracle nu răspunde.** `92.4.162.138:5678` și `:80` → timeout 20s. Control: `http://example.com` → 200. Cel mai probabil IP schimbat la reboot (gateway-ul servește `DEFAULT_IP` din iulie, blob-ul pare nescris) sau VM oprit.
- **Trigger zilnic „Daily Freelancer Scan" șters.** Rula din 25.08, 38–42 secunde per rulare, zero pagini SCAN produse. Sesiunile programate nu primesc conectori MCP, deci nu aveau acces la Notion.
- **`JOB INBOX` creat pe 25.08 era duplicat**, 0 rânduri, arhivat de Cătălin pe 31.08 cu eticheta „NU FOLOSI".
- **Cele trei audituri (Claude Code, ChatGPT, Perplexity) nu se suprapun deloc.** Intersecția listelor de „TERMINAT" este goală. Vezi `SKILL-MATRIX.md`.
