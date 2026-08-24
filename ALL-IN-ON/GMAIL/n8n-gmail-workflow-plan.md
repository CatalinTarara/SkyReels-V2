# Plan workflow n8n — Rutare automată Gmail

Data: 2026-08-24 · Status: DOCUMENTAT, neexecutat · Necesită: VM n8n accesibil

---

## Scop

Înlocuitor / complementar pentru filtrele native Gmail. Avantajul față de filtrele Gmail: poate face clasificare AI pentru mesajele ambigue și poate acționa mai complex (forward, Notion, Airtable).

---

## Arhitectura workflow-ului

```
Gmail Trigger (polling 5 min)
    ↓
Switch — clasificare pe From/Subject
    ↓
├── ALL-IN-ON/Alerte → label_thread (Label_51) + Notion notify
├── BILLING/Plăți Eșuate → label_thread (Label_57) + Telegram alert
├── BILLING/Facturi → label_thread (Label_58)
├── BILLING/Alerte → label_thread (Label_59)
├── SECURITATE/Login-Alerts → label_thread (Label_60) + Telegram alert
├── JOB-ALERTS/Upwork → label_thread (Label_55)
├── JOB-ALERTS/Remote → label_thread (Label_56)
├── AI-NEWS → label_thread (Label_43)
└── No match → rămâne în INBOX neethichetat
```

---

## Noduri necesare

| Nod | Configurare |
|---|---|
| Gmail Trigger | Event: New Email, Poll every: 5 minutes |
| IF / Switch | Condiții pe `from`, `subject` (regexp) |
| Gmail (Apply Label) | Operation: Add Label, Label ID: din tabel de mai jos |
| Telegram (opțional) | Chat ID: Catalin, Message: alert structurat |
| Notion (opțional) | Database: Inbox Log, operație: create entry |

---

## Label ID-uri pentru n8n

| Etichetă | Label ID Gmail |
|---|---|
| 📌 ALL-IN-ON | Label_38 |
| 📌 ALL-IN-ON/Notion | Label_48 |
| 📌 ALL-IN-ON/n8n | Label_49 |
| 📌 ALL-IN-ON/Airtable | Label_50 |
| 📌 ALL-IN-ON/Alerte | Label_51 |
| 💼 CLIENȚI | Label_39 |
| 💼 CLIENȚI/Leads | Label_52 |
| 💼 CLIENȚI/Activi | Label_53 |
| 💼 CLIENȚI/Oferte-Facturi | Label_54 |
| 🔍 JOB-ALERTS | Label_40 |
| 🔍 JOB-ALERTS/Upwork | Label_55 |
| 🔍 JOB-ALERTS/Remote | Label_56 |
| 💰 BILLING | Label_41 |
| 💰 BILLING/Plăți Eșuate | Label_57 |
| 💰 BILLING/Facturi | Label_58 |
| 💰 BILLING/Alerte | Label_59 |
| 🔒 SECURITATE | Label_42 |
| 🔒 SECURITATE/Login-Alerts | Label_60 |
| 📰 AI-NEWS | Label_43 |

---

## Reguli de clasificare (regexp pentru Switch node)

```javascript
// ALL-IN-ON/Alerte
from.match(/render\.com|hetzner|oracle|serpapi|n8n\.io/i)

// BILLING/Plăți Eșuate
from.match(/failed-payments@stripe|creativefabrica|emergentnet/i) ||
subject.match(/payment failed|unsuccessful|suspended.*non-payment/i)

// BILLING/Facturi
from.match(/stripe\.com|paddle\.com|nuelink|emergentnet/i) &&
subject.match(/invoice|receipt|subscription/i)

// BILLING/Alerte
subject.match(/searches are exhausted|quota|limit exceeded|API limit/i)

// SECURITATE/Login-Alerts
from.match(/accounts\.google\.com|noreply-accounts@google/i)

// JOB-ALERTS/Upwork
from.match(/upwork\.com|freelancer\.com/i)

// AI-NEWS
from.match(/openai\.com|anthropic\.com|huggingface\.co|perplexity\.ai/i)
```

---

## Extindere viitoare: clasificare AI

Pentru mesajele care nu se potrivesc la nicio regulă regexp, se poate adăuga un nod AI:

```
Mesaj neclasat
    ↓
OpenAI / Claude (prompt: clasifică în ALL-IN-ON/BILLING/JOB-ALERTS/SECURITATE/AI-NEWS/SKIP)
    ↓
Apply Label pe baza răspunsului
```

Cost estimat: ~0.001 USD/mesaj. Nu e necesar dacă filtrele regexp acoperă >90% din trafic.

---

## Precondiții de execuție

1. VM n8n accesibil (verificat via oracle_gateway → /n8n)
2. Credential Gmail OAuth activ în n8n (cu scope `gmail.modify`)
3. Label ID-urile de mai sus validate cu `list_labels` din Gmail API

---

*Workflow-ul se implementează după rezolvarea B2 (acces n8n).*
