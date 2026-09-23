# LAPTOP HANDOFF — ce rulezi local ca să închidem P0-urile

Data: 2026-09-23. Scop: sesiunea cloud nu ajunge la n8n, la Oracle VM sau la fișierele tale. Claude Code instalat pe laptop ajunge. Acest fișier e lista exactă de pași, în ordine.

Nimic de aici nu scrie secrete în repo. Exportul n8n se face **fără credențiale decriptate**.

---

## 0. Instalare (o singură dată)

```bash
npm install -g @anthropic-ai/claude-code
git clone https://github.com/CatalinTarara/SkyReels-V2.git
cd SkyReels-V2
git checkout claude/all-in-on-inventory-audit-rugq4i
claude
```

Primul mesaj către Claude local: *„Citește ALL-IN-ON/LAPTOP-HANDOFF.md și execută pașii 1–4, oprindu-te la fiecare pas marcat STOP."*

---

## 1. Află IP-ul real al VM-ului Oracle

Consola Oracle Cloud → Compute → Instances → instanța n8n → **Public IP**.

- Dacă e diferit de `92.4.162.138`: VM-ul a primit IP nou, gateway-ul e expirat.
- Dacă instanța e **Stopped**: pornește-o. Oracle Free Tier poate recupera instanțele oprite mult timp.

**STOP** — confirmă IP-ul înainte de pasul 2.

## 2. Verifică dacă n8n trăiește

```bash
ssh -i ~/.ssh/oci_ed25519 ubuntu@<IP_REAL> 'docker ps --format "{{.Names}}\t{{.Status}}"'
```

Așteptat: un container n8n `Up`. Dacă nu există: **STOP**, nu repornești nimic fără decizie.

## 3. Backup workflow-uri (C3 — cel mai mare risc)

```bash
ssh -i ~/.ssh/oci_ed25519 ubuntu@<IP_REAL> \
  'docker exec <container_n8n> n8n export:workflow --all --separate --output=/tmp/wf/ && \
   docker cp <container_n8n>:/tmp/wf /tmp/wf'
# NU în SkyReels-V2 — acel repo e PUBLIC (verificat 2026-09-23).
# Creează întâi un repo PRIVAT, ex. CatalinTarara/all-in-on-n8n, și clonează-l.
mkdir -p ~/all-in-on-n8n/workflows
scp -r -i ~/.ssh/oci_ed25519 ubuntu@<IP_REAL>:/tmp/wf/* ~/all-in-on-n8n/workflows/
```

Înainte de commit, Claude local verifică că niciun fișier nu conține tokenuri:

```bash
grep -rniE "api[_-]?key|token|secret|password|bearer" ~/all-in-on-n8n/workflows/ | head
```

Workflow-urile conțin doar **ID-uri** de credențiale, nu valorile. Dacă grep găsește valori literale (chei lipite direct în noduri HTTP): **STOP**, se redactează înainte de commit.

**`SkyReels-V2` este public** (fork public al proiectului SkyReels). Exportul n8n merge **exclusiv** în repo-ul privat de mai sus.

## 4. Test poarta Telegram (C4)

Trimite `/status` botului din Telegram. Notează: răspunde / nu răspunde. Nu repara nimic încă.

---

## După ce ai făcut 1–4

Trimite în sesiunea cloud (sau spune lui Claude local să actualizeze direct):

| Ce | Rezultat |
|---|---|
| IP real | … |
| Container n8n Up? | da / nu |
| Nr. workflow-uri exportate | … |
| `/status` Telegram | răspunde / nu |

Cu acestea actualizez `04-RISK-REGISTER.md`, `05-BACKLOG.md`, `SKILL-MATRIX.md` și pornim pasul următor: **TLS pe n8n (C2)** — plan deja scris în `AUDIT/tls-plan.md`.

## Ce NU face Claude local fără aprobarea ta explicită

- restart/stop container sau VM
- modificare workflow-uri n8n
- ștergeri
- orice trimitere de mesaje către terți
- orice acțiune pe SOLO sau Wingman
