# ALL IN ON — Backup n8n

**Status: AȘTEPTARE ACCES** · Data structurii: 2026-08-24

---

## Ce conține acest folder

`workflows/` — export complet al tuturor workflow-urilor n8n în format JSON.

Fiecare fișier JSON conține un workflow complet: noduri, conexiuni, credențiale referențiate (ID-uri, nu valori), setări de activare, trigger-e.

---

## Cum se face exportul

### Opțiunea A — n8n REST API (recomandat)

Necesită: API key din n8n Settings → API Keys.

```bash
# Înlocuiește N8N_URL și API_KEY
curl -H "X-N8N-API-KEY: <API_KEY>" \
     "http://<N8N_URL>/api/v1/workflows?limit=100" \
     | jq '.data[]' -c > workflows/export-$(date +%Y%m%d).json
```

### Opțiunea B — n8n CLI în Docker

Necesită: acces SSH la VM.

```bash
docker exec -u node n8n n8n export:workflow --all --output=/home/node/export/
# Copiem din container
docker cp n8n:/home/node/export/ ./workflows/
```

---

## Cum se face restore

```bash
# Import un singur workflow
n8n import:workflow --input=workflows/workflow-name.json

# Import toate
n8n import:workflow --separate --input=workflows/
```

---

## Frecvența recomandată

| Eveniment | Acțiune |
|---|---|
| Înainte de orice modificare majoră | Export manual |
| Săptămânal | Export automat (workflow n8n de backup) |
| Înainte de update n8n | Export obligatoriu |

---

## Ce NU conține acest backup

- Valorile credențialelor (chei API, tokens) — acestea sunt criptate în PostgreSQL
- Datele din PostgreSQL (execuții, logs)
- Configurația Docker / docker-compose.yml
- Variabilele de mediu (.env)

**Pentru un backup complet** este nevoie și de:
1. `pg_dump` al bazei PostgreSQL
2. Export credențiale (Settings → Credentials → Export, dacă e disponibil)
3. Fișierele de configurare Docker

---

*Structura creată de Claude Code. Workflow-urile se adaugă după furnizarea accesului.*
