# DECISIONS

Registru de decizii. O decizie intră aici doar când e **luată**, cu cine a luat-o.

## Luate

### D1 — Notion este source of truth pentru comunicarea între agenți
**Cine:** Catalin · **Când:** 2026-08-23
**Consecință:** Catalin nu mai transportă instrucțiuni între ChatGPT și Claude Code. Excepție: acțiunile care cer autorizarea lui explicită.

### D2 — Nu se creează structură Notion paralelă
**Cine:** Claude Code, conform instrucțiunii „nu crea duplicate" · **Când:** 2026-08-23
**Motiv:** `CiT AI Control Plane` + `CiT Master Tool & Channel Registry` acoperă deja rolul. S-a adăugat doar o pagină de status și un rând de task.

### D3 — SOLO rămâne zonă interzisă
**Cine:** Catalin (decizie fiscală anterioară, confirmată în registry)
**Consecință:** nu se închide abonamentul, nu se migrează la SmartBill, nu se modifică subscription. Facturare manuală, CUI 54604995.

---

## În așteptare — blochează execuția

### DEC-1 — Mecanismul de trezire Claude Code
**Cine decide:** Catalin
**Opțiuni:** (A) n8n lansează sesiuni prin webhook · (B) rutină recurentă care verifică Notion · (C) pornire manuală + golirea întregii cozi
**Recomandare Claude Code:** (C) acum, (B) după stabilizarea protocolului
**Blochează:** bucla automată ChatGPT → Notion → Claude Code

### DEC-2 — Motorul de distribuție: n8n nativ sau Blotato
**Cine decide:** Catalin
**Context:** Airtable are schema completă pentru Blotato (Account ID, targetType, idempotency), dar publicarea reală merge prin noduri native n8n pe 5 rețele. Blotato a produs 3 erori de publicare pe 17.08.
**Opțiuni:** (A) n8n rămâne motorul — zero cost, funcționează azi · (B) se construiește Blotato complet
**Regula:** nu ambele.

### DEC-3 — Abonamentele cu plăți eșuate
**Cine decide:** Catalin
**Context:** Render (sold neplătit, 3 notificări), Nuelink ($144 × 3 eșecuri), Emergent Labs (94.50 lei). Nuelink și Blotato par să acopere aceeași funcție.
**Opțiuni:** actualizare card SAU anulare deliberată, per serviciu.

### DEC-4 — Extinderea schemei Control Plane
**Cine decide:** Catalin (owner schema)
**Necesar:** statusuri `ACKNOWLEDGED`, `WAITING_APPROVAL`, `VERIFIED`, `FAILED`; câmpuri `EXECUTION_LOG`, `FILES_CHANGED`, `SYSTEMS_CHANGED`, `TESTS`.
**Fără ele:** protocolul cerut nu poate fi exprimat complet — lipsesc exact stările „aștept aprobare" și „am eșuat".
