# 01 — INVENTORY

Toate intrările au SOURCE. Nimic presupus.

## A. LOCAL
STATUS = **UNKNOWN (mediu greșit)**. Vezi 03-ACCESS-STATUS.md.
Singurul repo pe disc: `/home/user/SkyReels-V2` — fork public SkyReels-V2 (model video), **fără legătură cu ALL IN ON**.

## B. GITHUB
SOURCE: MCP `github.get_me`, `list_repos`

| Câmp | Valoare |
|---|---|
| User | `CatalinTarara` (Catalin Irinel Tarara, România), creat 2025-04-19 |
| Repo-uri publice | 1 |
| În scope sesiune | `CatalinTarara/SkyReels-V2` (fork, push OK) |
| Branch principal | `main` |
| Proiecte ALL IN ON pe GitHub | **NOT FOUND** |

Registry-ul tău Notion listează GitHub ca `NEEDS CREDENTIAL`, `Account: necunoscut`. Acum știm contul: `CatalinTarara`. Concluzia registry-ului rămâne validă: **nu există cod ALL IN ON versionat pe GitHub**.

## C. n8n / INFRASTRUCTURĂ
SOURCE: val.town `oracle_gateway/main.tsx`, Notion Master Registry, Gmail

| Element | Valoare | Status |
|---|---|---|
| Server | `catalin-n8n-server`, **Oracle Cloud VM** (Ubuntu) | VERIFIED indirect |
| IP | dinamic, urmărit de `oracle_gateway`; fallback hardcodat în cod | ACTIV |
| n8n UI | `http://<IP>:5678` — **HTTP simplu, fără TLS, fără reverse proxy** | ⚠️ |
| Acces | SSH cu cheia `~/.ssh/oci_ed25519` (pe mașina ta, nu aici) | BLOCKED de aici |
| Amploare | **8 departamente, 63 workflow-uri active, ~170 workflow-uri total** | din registry |
| REST API n8n | **401 — cheie lipsă din `user_api_keys`** | BLOCKED |
| CLI | `docker exec n8n` funcțional | VERIFIED (istoric) |
| n8n Cloud | workspace separat, magic link 2026-08-18 | UNKNOWN |
| PostgreSQL | referit (introspecție blocată de permisiuni) | UNKNOWN |
| Reverse proxy / TLS | **NOT FOUND** | ⚠️ |
| Backup | **NOT FOUND — nicio dovadă** | ⚠️ |

## D. AUTOMATIZĂRI SECUNDARE — Make.com
SOURCE: MCP `Make.scenarios_list` (team 1810705). Org **Free, `isPaused: true`**.

| Scenariu | Status | Note |
|---|---|---|
| 🏠 Prospector Imobiliare Prahova → Telegram | paused, valid | 6 execuții, 1138 op — singurul care a rulat |
| 📱 Content Creator AI → Facebook Daily | paused, **`isinvalid: true`** | 0 execuții |
| Social Media Comment Responder | paused, **`isinvalid: true`** | 0 execuții |
| CV Upwork Auto – GPT + Notion + Gmail | paused | 0 execuții, ultima editare 2025-07 |

⚠️ Registry-ul Notion zice „Make: ZERO scenarii". **Fals** — sunt 4. Registry-ul e depășit aici.
⚠️ „Prospector Imobiliare" e off-brand față de CiT Automation Architect.

## E. DATA LAYER — Airtable
SOURCE: MCP. Baze: `Social Media Automation` (favorit), `Social Media`, `Untitled Base`.

`Social Media Automation` — 6 tabele, schema matură:
- **Content Items** — 27 câmpuri: Platform Variants (JSON), Content Hash (SHA-256 anti-duplicat), Media URL public, Media Expires At, Approval Status (poartă umană), Avatar Clips (JSON)
- **Schedules**, **Platforms** (Blotato Account ID, targetType, Max Text Length, kill-switch `Publishing Enabled`), **Distribution Statuses** (Idempotency Key `contentId::platform::scheduledDate`, Error Category, Attempts, n8n Execution ID), **Video Jobs** (JSON2Video, Quality Score <80 = blocat), **Clipuri & Postări**

Acesta e cel mai bine proiectat element din tot sistemul: idempotență, retry, kill-switch, trasabilitate n8n, poartă de aprobare.

## F. CONTROL PLANE — Notion
SOURCE: MCP. Workspace `10dee1df-f25a-8117-a49b-00033a44b8f3`.
- **CiT AI Control Plane** (`da3c8305…`) — 5 task-uri, coada ChatGPT ↔ Claude Code ↔ n8n ↔ Catalin
- **CiT Master Tool & Channel Registry** (`31e6edd1…`) — **21 unelte**, cu Status/Blocker/Next Action

## G. WEB / FUNNEL
| Activ | Detaliu | SOURCE |
|---|---|---|
| Netlify `effulgent-cupcake-906882` → **freelancerhubpro.org** | deploy `ready`, forms enabled, `mfa_enabled=false`, og:image lipsă | MCP + registry |
| Netlify `gentle-mochi-f26534` | fără domeniu custom, scop neclar | MCP |
| val.town `freelancerhubpro` | endpoint HTTP public | MCP |
| val.town `oracle_gateway` | **gateway IP + redirect n8n, PUBLIC** | MCP |
| Replit `N8n Proxy` | monorepo pnpm/TS, Express 5, PostgreSQL+Drizzle, Zod, Orval | MCP |
| Supabase `Access Shared Link` | eu-west-2, PG 17, **INACTIVE** | MCP |
| Lovable | workspace gol | MCP |

## H. DISTRIBUȚIE & MEDIA
| Serviciu | Status | SOURCE |
|---|---|---|
| Blotato | cont activ; **3 erori de publicare** (LinkedIn, YouTube, Instagram) 2026-08-17 | Gmail |
| JSON2Video | activ, primul render 2026-08-13 | Gmail |
| HeyGen / HyperFrames | conector MCP activ | Gmail + conectori |
| Metricool | **TikTok deconectat** (acces revocat) | Gmail |
| Nuelink | **abonament $144 eșuat de 3× consecutiv** | Gmail/Stripe |

## I. PLĂȚI / FACTURARE
| Element | Status | SOURCE |
|---|---|---|
| Stripe (cont propriu) | **NEACTIVAT** — „Complete your business profile to unlock live payment processing"; „Review your account representative" | Gmail |
| SOLO (falcon.solo.ro), CUI 54604995 | VERIFIED — facturare manuală, ZONĂ INTERZISĂ | registry |
| Render | **sold neplătit**, 3 notificări | Gmail |
| Emergent Labs | plată 94.50 lei eșuată | Gmail |
| Gumroad | **NOT FOUND** — zero dovezi | Gmail |

## J. CANALE (din Master Registry)
P0: Telegram @CatalinBot (3 triggere pe același bot), LinkedIn (editare blocată), Upwork (portofoliu greșit), Freelancer.com, Notion, n8n
P1: WhatsApp Cloud API, X/Twitter, e-Matrix, Wingman, Airtable, Netlify, GitHub, Blotato
P2/P3: Reddit, Pinterest, Threads, Metricool, Make, SOLO
