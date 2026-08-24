# Reguli filtre Gmail — de introdus manual

Data: 2026-08-24 · Introdu aceste filtre în: Gmail → Settings → See all settings → Filters and Blocked Addresses → Create a new filter

---

## Cum se adaugă un filtru

1. Gmail → Settings (⚙️) → See all settings
2. Tab **Filters and Blocked Addresses**
3. **Create a new filter**
4. Completezi câmpul „From" sau „Subject" → **Create filter**
5. Bifezi **Apply the label** → selectezi eticheta → **Create filter**

---

## Regulile (în ordinea priorității)

### 1. ALL-IN-ON/Alerte — alerte de infrastructură

**From:** `render.com OR hetzner.com OR oracle.com OR oraclecloud.com OR serpapi.com OR n8n.io`
**Etichetă:** `📌 ALL-IN-ON/Alerte`

---

### 2. ALL-IN-ON/n8n — comunicări n8n

**From:** `n8n.io`
**Subject:** `n8n`
**Etichetă:** `📌 ALL-IN-ON/n8n`

---

### 3. ALL-IN-ON/Notion — comunicări Notion

**From:** `notion.so`
**Etichetă:** `📌 ALL-IN-ON/Notion`

---

### 4. ALL-IN-ON/Airtable — comunicări Airtable

**From:** `airtable.com`
**Etichetă:** `📌 ALL-IN-ON/Airtable`

---

### 5. BILLING/Plăți Eșuate — plăți eșuate

**From:** `failed-payments@stripe.com OR noreply@app.emergentnet.io OR hi@creativefabrica.com`
**Subject:** `payment failed OR unsuccessful OR suspended due to non-payment OR invoice`
**Etichetă:** `💰 BILLING/Plăți Eșuate`

Bifează și: **Mark as important**

---

### 6. BILLING/Facturi — facturi și abonamente

**From:** `stripe.com OR paddle.com OR nuelink.com OR emergentnet.io`
**Subject:** `invoice OR receipt OR subscription`
**Etichetă:** `💰 BILLING/Facturi`

---

### 7. BILLING/Alerte — limite API și cotă

**Subject:** `searches are exhausted OR quota OR limit exceeded OR API limit`
**Etichetă:** `💰 BILLING/Alerte`

---

### 8. SECURITATE/Login-Alerts — alerte de conectare Google

**From:** `accounts.google.com OR no-reply@accounts.google.com OR noreply-accounts@google.com`
**Etichetă:** `🔒 SECURITATE/Login-Alerts`

Bifează și: **Never send it to Spam**, **Mark as important**

---

### 9. JOB-ALERTS/Upwork — alerte Upwork

**From:** `upwork.com OR freelancer.com`
**Etichetă:** `🔍 JOB-ALERTS/Upwork`

---

### 10. JOB-ALERTS/Remote — oportunități remote

**Subject:** `remote job OR hiring OR we found OR job alert OR job opportunity`
(Adaugă `-from:upwork.com` în câmpul „Doesn't have" pentru a evita duplicatele)
**Etichetă:** `🔍 JOB-ALERTS/Remote`

---

### 11. AI-NEWS — știri și update-uri AI

**From:** `openai.com OR anthropic.com OR huggingface.co OR perplexity.ai`
**Subject:** `AI newsletter OR LLM OR GPT OR Claude`
**Etichetă:** `📰 AI-NEWS`

---

## Note

- Gmail aplică filtrele pe mesajele **noi** după ce le creezi — nu retroactiv
- Pentru mesajele deja existente, la pasul „Create filter" bifează și **Also apply filter to matching conversations**
- Filtrele pot fi combinate sau separate — depinde de cât de precis vrei să fie matching-ul
- Dacă un mesaj se potrivește la mai multe filtre, Gmail aplică **toate** etichetele

---

*Fișier generat de Claude Code pe baza ierarhiei de etichete create în sesiunea 2026-08-24.*
