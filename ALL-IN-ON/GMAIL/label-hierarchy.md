# Ierarhia etichetelor Gmail — ALL IN ON

Data: 2026-08-24 · Stare: IMPLEMENTAT

---

## Structura completă

| Etichetă | Label ID | Culoare | Scop |
|---|---|---|---|
| 📌 ALL-IN-ON | Label_38 | BLUE | Parent — infrastructură și automatizări |
| 📌 ALL-IN-ON/Notion | Label_48 | BLUE | Email-uri de la Notion |
| 📌 ALL-IN-ON/n8n | Label_49 | BLUE | Email-uri despre n8n |
| 📌 ALL-IN-ON/Airtable | Label_50 | BLUE | Email-uri de la Airtable |
| 📌 ALL-IN-ON/Alerte | Label_51 | BLUE | Alerte infrastructură (SerpApi, Oracle, Render, etc.) |
| 💼 CLIENȚI | Label_39 | GREEN | Parent — relații clienți |
| 💼 CLIENȚI/Leads | Label_52 | GREEN | Prospecți noi |
| 💼 CLIENȚI/Activi | Label_53 | GREEN | Clienți activi |
| 💼 CLIENȚI/Oferte-Facturi | Label_54 | GREEN | Oferte trimise și facturi |
| 🔍 JOB-ALERTS | Label_40 | ORANGE | Parent — oportunități de lucru |
| 🔍 JOB-ALERTS/Upwork | Label_55 | ORANGE | Alerte Upwork și Freelancer |
| 🔍 JOB-ALERTS/Remote | Label_56 | ORANGE | Alte oportunități remote |
| 💰 BILLING | Label_41 | RED | Parent — plăți și facturare |
| 💰 BILLING/Plăți Eșuate | Label_57 | RED | Plăți eșuate (Stripe, Nuelink, Emergent, etc.) |
| 💰 BILLING/Facturi | Label_58 | RED | Facturi și receipturi |
| 💰 BILLING/Alerte | Label_59 | RED | Limite API, cotă epuizată (SerpApi, etc.) |
| 🔒 SECURITATE | Label_42 | DARK_RED | Parent — alerte de securitate |
| 🔒 SECURITATE/Login-Alerts | Label_60 | DARK_RED | Conectări noi Google, OAuth grants |
| 📰 AI-NEWS | Label_43 | PURPLE | Știri și update-uri AI (OpenAI, Anthropic, HuggingFace) |

---

## Ce s-a șters

34 etichete goale (duplicate create de Wingman/n8n anterior) — toate aveau 0 mesaje:
Label_1 prin Label_20, Label_22 prin Label_32, Label_34 prin Label_36.

---

## Mesaje migrate

| Mesaj | Eticheta veche | Eticheta nouă |
|---|---|---|
| SerpApi searches exhausted (19f9d024) | Label_21 (🔔 API-LIMIT) | Label_59 (BILLING/Alerte) |
| Nuelink $144 payment failed × 2 (Label_33) | Label_33 (🚨 URGENT - PAYMENT FAILED) | Label_57 (BILLING/Plăți Eșuate) |
| 10+ alerte Google login (inbox) | — | Label_60 (SECURITATE/Login-Alerts) |
| 7+ plăți eșuate Stripe/Emergent/Netlify (inbox) | — | Label_57 (BILLING/Plăți Eșuate) |
| Anthropic + OpenAI email (inbox) | — | Label_43 (AI-NEWS) |

Notă: Label_33 și Label_37 (⚠️ Backup Needed) sunt în TRASH — mesajele au fost mutate în noile etichete, labelele vechi rămân ca referință până la golirea coșului.

---

## Pasul următor (manual, Catalin)

Introdu filtrele din `filter-rules.md` în Gmail Settings pentru ca mesajele **noi** să fie etichetate automat.
Estimat: 5–10 minute.

---

*Implementat de Claude Code în sesiunea 2026-08-24.*
