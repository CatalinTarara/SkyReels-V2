# CHATGPT → CLAUDE

> Cache local al task-urilor cu `From=ChatGPT`, `To=Claude Code` din Control Plane.
> **Notion rămâne sursa comună.** Nu edita aici ce trebuie să ajungă la ChatGPT — scrie în Notion.

Sursa: [CiT AI Control Plane](https://app.notion.com/p/da3c83051ea74545b95b708fcf400d2d) (`da3c8305-1ea7-4545-b95b-708fcf400d2d`)

---

## Task-uri primite de la ChatGPT

### CIT-1 — Establish ChatGPT ↔ Claude Code Control Plane
**Status:** Done · **Priority:** P0
**Blocker (istoric):** baza ChatGPT (`27d4734a…`) și HQ-ul (`3be332a2…`) inaccesibile din acest workspace.
**Rezultat:** bridge-ul există. Confirmarea bidirecțională depinde încă de punctul deschis de mai jos.

### CIT-4 — CIT-BRIDGE-002 — Notion → n8n → Telegram trigger
**Status:** Done · **Priority:** P0 · **VERIFIED** (execuții 84364, 84380)
**Cauza reală identificată:** nu permisiunea Notion, ci (1) trei noduri HTTP Notion fără credențial atașat → `Credentials not found`; (2) credențialul „Notion Agency" conținea un secret OAuth în loc de Internal Integration Token → 401.
**Fix aplicat (istoric):** integrare Notion `n8n Control Plane`, token pus direct în credențialul existent. Zero credențiale noi, zero workflow-uri noi.

---

## PUNCT DESCHIS — blochează handshake-ul real

**ChatGPT scrie în alt workspace.**
- ChatGPT folosește: `27d4734a…`
- Sursa reală este: `da3c8305-1ea7-4545-b95b-708fcf400d2d` (workspace `10dee1df-f25a-8117-a49b-00033a44b8f3`)
- Fetch încrucișat → 404

Până când ChatGPT citește și scrie în baza corectă, cele două jumătăți ale planului de control nu se văd, iar „colaborarea" rămâne transport manual prin Catalin.

**Verificare propusă:** dacă ChatGPT poate citi rândul CIT-6 (`CIT-AUDIT-001`), bridge-ul e confirmat bidirecțional.

---

## LIMITARE STRUCTURALĂ DE PROTOCOL

O sesiune Claude Code **nu rulează continuu**. Este pornită, rulează, se termină. Nu poate face polling pe Notion între sesiuni.

Deci fluxul „ChatGPT scrie în Notion → Claude Code observă și execută singur" **nu funcționează așa cum e desenat**, oricâtă infrastructură Notion s-ar adăuga. Ceva trebuie să *pornească* sesiunea:

- **(A)** n8n lansează o sesiune Claude Code remote prin webhook la fiecare task nou — necesită integrare
- **(B)** rutină recurentă pe partea Claude Code care verifică Notion la interval fix — fezabil azi
- **(C)** Catalin pornește sesiunea, iar Claude Code golește apoi întreaga coadă fără altă intervenție — fezabil azi, zero construcție

**Recomandare:** (C) acum, (B) după ce protocolul e stabil.
Chiar și cu (C), Catalin nu mai e mesager — doar declanșator.
