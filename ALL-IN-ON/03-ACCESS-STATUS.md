# 03 — ACCESS STATUS

Data: 2026-08-23 · Cont: catalin.tarara.remote@gmail.com
Mediu: container remote Claude Code (efemer). Regim: **READ-ONLY**.

## Legendă
VERIFIED = confirmat prin apel reușit · BLOCKED = acces refuzat/indisponibil · NOT FOUND = căutat, nu există · UNKNOWN = neverificabil de aici

| Sursă | Status | Metodă de acces | Dovadă (SOURCE) |
|---|---|---|---|
| Filesystem local (`/home/catalintarara`, `~/ALL-IN-ON`, Desktop, Documente, Descărcări) | **NOT APPLICABLE / UNKNOWN** | — | `ls /home` → doar `claude`, `ubuntu`, `user`. HOME=`/root`. Mașina ta nu e acest container. |
| Repo-uri Git pe disc | VERIFIED | `find / -name .git` | Doar `/home/user/SkyReels-V2` (+ `/opt/rbenv`, `/opt/nvm` = infra) |
| Fișiere `*all-in-on*` / `*n8n*` pe disc | NOT FOUND | `find / -iname` | Zero rezultate |
| SSH | NOT FOUND | `ls -la ~/.ssh` | Director gol. Git merge pe HTTPS + token efemer. |
| GitHub | **VERIFIED (scope limitat)** | MCP `github` | `get_me` → `CatalinTarara`; `list_repos` → 1 repo: `CatalinTarara/SkyReels-V2` |
| n8n (server) | **BLOCKED** | — | Niciun conector n8n; fără SSH; fără API key. Existența confirmată indirect. |
| Oracle Cloud VM | **BLOCKED** | — | Confirmată prin codul val.town `oracle_gateway` + registry Notion |
| Notion | VERIFIED | MCP | `CiT AI Control Plane` + `CiT Master Tool & Channel Registry` citite integral |
| Airtable | VERIFIED | MCP | 3 baze, schema completă citită |
| Make.com | VERIFIED | MCP | org 3812270, team 1810705, 4 scenarii |
| Netlify | VERIFIED | MCP | 2 site-uri, deploy `ready` |
| Replit | VERIFIED | MCP | app `N8n Proxy`, fișiere listate |
| val.town | VERIFIED | MCP | 2 vals, cod citit |
| Supabase | VERIFIED (proiect INACTIVE) | MCP | `qirzdpexofoytgqqosye`, eu-west-2 |
| Lovable | VERIFIED (gol) | MCP | workspace `Catalin's Lovable`, 0 proiecte |
| Gmail / Drive / Calendar | VERIFIED | MCP | căutări executate |
| Stripe | **PARȚIAL** | MCP încărcat, neinterogat | Dovadă din Gmail: 2 conturi distincte (`acct_1AMcez…`, `acct_1QMlxN…`) |
| Render | **BLOCKED** | fără conector | 3 e-mailuri „Invalid payment info" |
| Blotato | BLOCKED | fără conector | e-mailuri login + 3 erori de publicare |
| HeyGen / HyperFrames | VERIFIED (conector activ) | MCP | „HyperFrames MCP is connected" |
| JSON2Video | BLOCKED | fără conector | e-mail „first video is ready" |
| Metricool | BLOCKED | fără conector | e-mail „unable to connect TikTok" |
| Upwork | PARȚIAL | MCP (doar search/job post) | fără acces la portofoliul propriu |
| Telegram | BLOCKED | fără conector | doar prin n8n |
| WhatsApp Cloud API | BLOCKED | fără conector | registry Notion |
| Tropic | **BLOCKED** | necesită OAuth | sesiune non-interactivă — nu se poate autoriza de aici |

## Acces care LIPSEȘTE și blochează auditul complet
1. n8n — URL instanță + API key (sau SSH la Oracle VM)
2. Filesystem local
3. Render — dashboard
4. Blotato — API key
5. Repo-uri GitHub private / alte organizații
