# ALL IN ON — Ce avem de fapt și ce ne trebuie

Data: 2026-08-24 · Bazat pe: audit cloud complet (08-23) + toate conversațiile sesiunii

---

## 1. Ce avem acum (verificat)

| Componentă | Stare | Unde rulează |
|---|---|---|
| n8n (~170 workflows, 63 active) | LIVE (sau Just-in-time Always Free) | Oracle Cloud VM |
| Notion Control Plane | LIVE | Cloud Notion |
| Airtable (3 baze, schema matură) | LIVE | Airtable cloud |
| freelancerhubpro.org | LIVE | Netlify |
| CIT-BRIDGE-002 (Notion→Telegram) | VERIFIED | n8n pe VM |
| oracle_gateway (IP tracker) | LIVE, PUBLIC, RISC C1 | val.town |
| Make (4 scenarii, toate pauzate) | PAUZAT | Make cloud |
| Replit (N8n Proxy app) | LIVE | Replit cloud |
| Supabase | INACTIV | Supabase cloud |

---

## 2. Ce lipsește critic (fără acestea sistemul e fragil sau incomplet)

### 2A — Infrastructură de supraviețuire (P0)

| Ce lipsește | Impact | Acțiune minimă |
|---|---|---|
| Backup n8n workflows | Pierdere totală ireversibilă dacă VM dispare | Export JSON → repo privat |
| oracle_gateway privatizat | Secret + IP VM expus public oricui | Setează val.town pe private, rotește secretul |
| TLS pe n8n | Credențiale în clar pe rețea | Reverse proxy (Caddy/Nginx) pe VM |

### 2B — Funcționalitate lipsă (P1)

| Ce lipsește | Impact | Unde e gaura |
|---|---|---|
| CRM real | Lead-urile WhatsApp intră și nu sunt procesate | WhatsApp → n8n e pass-through de 3 noduri |
| Stripe activat | Nu poți încasa online | Stripe dashboard — KYC necomplet |
| Bot Telegram deduplicat | 3 triggere pe un bot = calcă pe coada celuilalt | n8n: consolidare triggere |
| Backup credențiale n8n | Dacă VM moare, pierzi toate integrările | Export manual sau Vault |

### 2C — Claritate arhitecturală (P2, blochează deciziile)

| Decizie amânată | Ce blochează |
|---|---|
| DEC-2: n8n nativ vs Blotato | Distribuția pe social nu e clară ca motor |
| DEC-3: ce abonamente se păstrează | Nuelink $144×3 eșuat, Render sold neplătit |
| DEC-1: cum pornește sesiunea Claude Code automat | Bucla ChatGPT→Notion→Claude nu e completă |

---

## 3. Ce e planificat dar neimplementat (atenție: nu confunda planul cu realitatea)

- **Blotato**: Airtable are schema completă pentru Blotato (Account ID, targetType, idempotency) — dar Blotato **nu e conectat**. 3 erori de publicare pe 17.08. Decizie necesară: n8n nativ (funcționează azi) sau Blotato complet.
- **Lead scoring / clasificare WhatsApp**: schema există în cap, nu în cod.
- **Portofoliu Upwork**: menționat în registry ca actualizat — neverificabil din cloud.
- **Lovable (gentle-mochi-f26534)**: scop neidentificat, poate fi duplicate sau experiment abandonat.

---

## 4. Oracle Cloud — situația reală

**Nu crea cont nou acum.**

- Trial a expirat 2026-08-24 03:51 UTC — asta elimină doar creditele plătite.
- Always Free include permanent 2× AMD Micro VM (1 OCPU, 1 GB RAM) — VM-ul curent se califică.
- Email de recuperare trimis la Oracle Support — răspuns așteptat 24–48h.
- **Test imediat**: deschide `oracle_gateway` → `/n8n` în browser. Dacă n8n se încarcă, VM-ul e viu pe Always Free.
- Dacă VM-ul e mort și Oracle nu răspunde în 48h → atunci discutăm recuperare sau cont nou.

**Ce se poate recupera dacă VM-ul e accesibil:**
- Toate workflow-urile n8n (din Docker volume)
- PostgreSQL cu datele
- Credențialele n8n (encrypt-ate, dar exportabile)

**Ce se pierde dacă VM-ul e șters definitiv și nu există backup:**
- Tot. Ireversibil. Acesta e exact riscul C3 din audit.

---

## 5. Gmail connector — blocat temporar

Token OAuth Gmail expirat. Claude Code nu poate reautoriza din sesiune cloud non-interactivă.

**Pentru a debloca:**
1. claude.ai → Settings → Connectors → Gmail → Disconnect
2. Reconnect → autorizează din nou OAuth
3. Revino în sesiune — ștergerea alertelor „Conectare nouă" va funcționa automat

---

## 6. Ce conectori sunt disponibili acum vs ce ar trebui

| Conector | Stare | Rolul în ALL IN ON |
|---|---|---|
| Gmail | CONECTAT (token expirat) | Monitorizare, alerte, draft-uri |
| Google Drive | CONECTAT | Stocare fișiere, exporturi |
| Google Calendar | CONECTAT | Scheduling, task timing |
| Notion | CONECTAT | Control Plane — sursă de adevăr |
| Canva | CONECTAT | Creație vizuală |
| Figma | CONECTAT | Design |
| Asana | CONECTAT | Project tracking (opțional, Notion acoperă) |
| Slack | NECONECTAT | Utilitar pentru notificări echipă (low priority) |
| Microsoft 365 | NECONECTAT | Nu e în stack-ul curent |
| Atlassian Rovo | NECONECTAT | Nu e în stack-ul curent |

**Concluzie conectori**: tot ce e critic pentru ALL IN ON (Notion, Gmail, Drive) e conectat. Slack ar fi util dar nu blocant.

---

## 7. Prioritățile reale (ce să faci în ordinea asta)

1. **Verifică dacă VM-ul e viu** — oracle_gateway → /n8n (2 minute, zero risc)
2. **Privatizează oracle_gateway + rotește secretul** (5 minute, risc maxim eliminat)
3. **Exportă workflow-urile n8n** dacă VM-ul e viu (citire pură, zero risc)
4. **Reconnectează Gmail** ca să poți șterge alertele și monitoriza inbox-ul
5. **Decide DEC-3** — actualizezi cardul sau anulezi Nuelink/Render/Emergent
6. **Decide DEC-2** — n8n nativ vs Blotato (afectează cum construiești mai departe)

---

*Fișier derivat din `06-ALL-IN-ON-MASTER-AUDIT.md` și conversațiile din sesiunea 2026-08-23/24.*
*Modificări operaționale efectuate: ZERO.*
