# Plan TLS pentru n8n — C2

Data: 2026-08-24 · Status: DOCUMENTAT, neexecutat · Necesită aprobare Catalin

---

## Situația actuală (problemă C2)

n8n rulează pe `http://92.4.162.138:5678` fără TLS.

**Consecința concretă**: orice credențial introdus în n8n (token Telegram, cheie Notion, token OpenAI, token Airtable) tranzitează rețeaua în clar. Un MITM între browser și VM poate captura totul.

**De ce contează acum**: `oracle_gateway` expune IP-ul VM-ului public. Cineva care știe IP-ul poate vedea traficul n8n dacă e pe același segment de rețea sau dacă reușește să se poziționeze în path.

---

## Soluția recomandată: Caddy reverse proxy

### De ce Caddy și nu Nginx/Apache

- Zero configurare SSL: Caddy obține și reînnoiește automat certificatele Let's Encrypt
- O singură comandă de instalare pe Ubuntu/Debian
- Un singur fișier de configurare (Caddyfile), mai simplu decât nginx.conf
- Funcționează cu IP dinamic dacă folosim un domeniu (freelancerhubpro.org sau un subdomain)

### Precondiție: domeniu DNS

TLS cu Let's Encrypt necesită un domeniu DNS care să pointeze la IP-ul VM-ului.

**Opțiuni**:
- `n8n.freelancerhubpro.org` → A record → IP VM (actualizat automat de oracle_gateway sau manual)
- Un subdomain dedicat (ex. `automation.freelancerhubpro.org`)

Fără domeniu DNS nu funcționează Let's Encrypt. Alternativă: certificat self-signed (nu recomandat pentru producție).

---

## Pași de execuție (după aprobare)

### 1. Instalare Caddy pe VM

```bash
# Pe Oracle VM (Ubuntu), ca root sau cu sudo
apt install -y debian-keyring debian-archive-keyring apt-transport-https curl
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | tee /etc/apt/sources.list.d/caddy-stable.list
apt update
apt install caddy
```

### 2. Caddyfile

```
n8n.freelancerhubpro.org {
    reverse_proxy localhost:5678
}
```

Caddy preia automat TLS de la Let's Encrypt pentru domeniul specificat.

### 3. Deschide portul 443 în Oracle Cloud Security List

Oracle Cloud blochează porturile implicit. Trebuie adăugat:
- Ingress rule: TCP port 443, source 0.0.0.0/0

```bash
# Și în firewall-ul Ubuntu de pe VM
iptables -I INPUT -p tcp --dport 443 -j ACCEPT
iptables -I INPUT -p tcp --dport 80 -j ACCEPT  # necesar pentru ACME challenge
```

### 4. Actualizare n8n pentru URL-ul nou

În `.env` sau `docker-compose.yml` al n8n, variabilele de actualizat:
```
N8N_HOST=n8n.freelancerhubpro.org
N8N_PORT=5678
N8N_PROTOCOL=https
WEBHOOK_URL=https://n8n.freelancerhubpro.org/
```

Fără `WEBHOOK_URL` corect, webhooks-urile (Telegram, etc.) continuă să returneze URL-uri HTTP în loc de HTTPS.

### 5. Restart n8n

```bash
cd /path/to/docker-compose/
docker compose down && docker compose up -d
```

### 6. Actualizare oracle_gateway

Val-ul `oracle_gateway` returnează IP-ul VM-ului. După TLS, nu mai e nevoie de IP direct — URL-ul n8n devine `https://n8n.freelancerhubpro.org`. Val-ul poate rămâne ca fallback pentru SSH.

### 7. Actualizare webhook-uri active în n8n

Webhook-urile active care trimit URL HTTP în Telegram/Notion trebuie resetate după schimbarea `WEBHOOK_URL`. n8n regenerează URL-urile la restart dacă variabila e setată corect.

---

## Verificare după execuție

```bash
curl -I https://n8n.freelancerhubpro.org/  # trebuie să returneze 200 sau redirect
# Verifică certificatul în browser: lacăt verde, emis de Let's Encrypt
```

Și în n8n: Settings → Webhooks → verifică că URL-urile afișate sunt HTTPS.

---

## Riscuri în execuție

| Risc | Probabilitate | Mitigare |
|---|---|---|
| Caddy nu obține certificat (DNS nepropadat) | Medie | Aștepți 5–15 min propagare DNS sau folosești `--staging` pentru test |
| n8n nu pornește cu `WEBHOOK_URL` greșit | Mică | Testezi mai întâi fără `WEBHOOK_URL`, adaugi după |
| Portul 443 blocat în Oracle Security List | Mare dacă nu e deschis | Verifici Security List din OCI Console înainte |
| Webhook-uri Telegram se strică | Medie | Le resetezi din n8n după restart (Tools → Webhook URLs) |

---

## Ce nu se schimbă

- n8n continuă să ruleze pe portul intern :5678
- Docker și PostgreSQL nu sunt atinse
- Workflow-urile nu sunt modificate
- Credențialele existente rămân valabile

---

**Status**: NEEXECUTAT. Necesită: (1) aprobare Catalin, (2) domeniu DNS configurat, (3) acces SSH la VM.
