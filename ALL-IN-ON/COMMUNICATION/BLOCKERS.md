# BLOCKERS

Stare la 2026-08-23. Fiecare intrare are cauză verificabilă, nu presupunere.

## B1 — Acces local absent (blochează auditul infrastructurii reale)
**Cauză:** sesiunea rulează într-un container cloud efemer. `/home/catalintarara`, `~/ALL-IN-ON`, Desktop, Documente, Descărcări nu există aici.
**Consecință:** filesystem local = `UNKNOWN`, nu „inexistent".
**Deblocare:** rulează promptul de audit dintr-o sesiune Claude Code pe laptop.

## B2 — n8n inaccesibil
**Cauză:** fără conector n8n, fără SSH (`~/.ssh` gol), fără API key. REST API returnează 401 — cheia lipsește din `user_api_keys`.
**Consecință:** toate datele despre workflow-uri sunt `HISTORICAL INFORMATION` din Master Registry, nu `VERIFIED NOW`.
**Deblocare:** cheie API n8n emisă din Settings, sau acces SSH la Oracle VM.

## B3 — ChatGPT pe workspace divergent
**Cauză:** ChatGPT scrie în `27d4734a…`; sursa reală e `da3c8305…`. Fetch încrucișat = 404.
**Consecință:** handshake-ul nu e confirmat bidirecțional.
**Deblocare:** ChatGPT trece pe baza corectă și confirmă că poate citi CIT-6.

## B4 — Nu există canal Notion → Claude Code
**Cauză:** limitare structurală, nu configurație. Sesiunile Claude Code nu rulează continuu.
**Consecință:** automatizarea completă a buclei nu e posibilă fără mecanism de trezire.
**Deblocare:** decizia Catalin între (A) webhook n8n, (B) rutină recurentă, (C) pornire manuală + golirea cozii.

## B5 — Servicii fără conector
Render · Blotato · JSON2Video · Metricool · Telegram · WhatsApp Cloud API · e-Matrix · Wingman.
**Consecință:** status `BLOCKED`, dovezi doar indirecte (Gmail, registry).
**Deblocare:** conectori sau chei API, per serviciu.

## B6 — Tropic necesită OAuth
**Cauză:** serverul MCP cere autorizare; sesiunea e non-interactivă.
**Deblocare:** autorizare din setările de conectori claude.ai sau dintr-o sesiune interactivă.

---

## B7 — Gmail OAuth token expirat (blochează auto-delete alerte)
**Cauză:** connector Gmail apare conectat în UI dar token OAuth a expirat separat.
**Consecință:** Claude Code nu poate trimite/șterge email-uri; nu poate reautoriza din sesiune cloud non-interactivă.
**Deblocare:** claude.ai → Settings → Connectors → Gmail → Disconnect → Reconnect → autorizează OAuth din nou.

---

## Blocaje rezolvate în această sesiune

- **Commit/push Git** — blocate inițial de clasificatorul de permisiuni; au trecut la reîncercare. Commit `0f162cf` împins pe `claude/all-in-on-inventory-audit-rugq4i`.
