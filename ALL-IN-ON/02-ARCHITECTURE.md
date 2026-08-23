# 02 — ARCHITECTURE

## CURRENT STATE (dedus din dovezi)

```
                    ┌─────────────────────────┐
                    │  Notion Control Plane   │  ← ChatGPT / Claude Code / Catalin
                    │  CiT AI Control Plane   │
                    │  Master Registry (21)   │
                    └───────────┬─────────────┘
                                │ (poll 15 min, workflow CIT-BRIDGE-002)
                                ▼
   Telegram @CatalinBot ◄──► ┌──────────────────────────────┐
   (command center +         │   n8n — Oracle Cloud VM      │
    poartă aprobare,         │   Docker, :5678 HTTP simplu  │
    22 workflow-uri)         │   63 active / ~170 total     │
                             │   8 "departamente"           │
                             └───┬──────┬──────┬────────┬───┘
                                 │      │      │        │
              ┌──────────────────┘      │      │        └────────────┐
              ▼                         ▼      ▼                     ▼
    ┌──────────────────┐     ┌────────────┐  ┌──────────────┐  ┌──────────┐
    │ Airtable         │     │ JSON2Video │  │ WhatsApp     │  │ 5 rețele │
    │ Social Media     │     │ + HeyGen   │  │ Cloud API    │  │ sociale  │
    │ Automation       │     │ (video)    │  │ (pass-thru)  │  │ (noduri  │
    │ (content+status) │     └────────────┘  └──────────────┘  │  native) │
    └────────┬─────────┘                                       └──────────┘
             │ (schema pregătită pentru Blotato — NEIMPLEMENTAT)
             ╎ - - - - - - ► Blotato (PLAN, nu integrare)
             │
             ▼
    freelancerhubpro.org (Netlify)  ◄── val.town freelancerhubpro
                                          val.town oracle_gateway ──► redirect :5678

  LATERAL / DECUPLAT:
    Make.com (4 scenarii, TOATE pe pauză) — nelegat de n8n
    Replit "N8n Proxy" (monorepo TS+Postgres) — rol neclar
    Supabase (INACTIVE) — nefolosit
    Lovable (gol) · GitHub (fără cod ALL IN ON) · Render (neplătit)
```

## Observația centrală

Sistemul are **un singur creier (n8n pe o VM Oracle) și niciun plan de recuperare**. Tot restul — Airtable, Notion, Telegram, social — sunt periferice care depind de el. Dacă VM-ul dispare, ~170 workflow-uri dispar cu el: nu există backup dovedit, nu există cod versionat pe GitHub, nu există export.

Al doilea tipar: **schema depășește implementarea**. Airtable are un design de nivel producție (idempotență, retry, kill-switch, QA video) pentru un motor de distribuție — Blotato — care, conform propriului tău registry, „este un PLAN, nu o integrare". Publicarea reală se face prin noduri native n8n. Deci o parte din cel mai bun design pe care îl ai nu e conectată la nimic.

Al treilea: **trei planuri de control care nu se văd între ele** — Notion (2 workspace-uri diferite: al tău vs. al ChatGPT), Telegram (3 triggere pe un bot care acceptă unul singur), Make (pauzat, invizibil în registry).

## TARGET STATE (propunere — NEIMPLEMENTAT)

```
GitHub (source of truth: workflow JSON exportat, IaC, docs)
   │  ├─ export automat n8n → git, zilnic
   ▼
n8n în spatele reverse proxy cu TLS (Caddy/nginx), :5678 ÎNCHIS public
   │  ├─ API key emisă → automatizări de mentenanță
   │  ├─ backup Postgres + volume, off-site, testat prin restore
   ▼
Un singur plan de control (Notion) · un singur bot Telegram · un singur data layer (Airtable)
   │
   ▼
DECIZIE distribuție: n8n nativ (zero cost, merge azi) SAU Blotato — nu ambele
```

Diferența cheie față de azi: sursa de adevăr se mută de pe o VM într-un repo, iar n8n devine ceva ce poate fi reconstruit, nu ceva ce poate fi pierdut.
