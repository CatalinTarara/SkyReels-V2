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

### D4 — DEC-1…DEC-4 se amână până la Master Audit unic
**Cine:** Catalin · **Când:** 2026-08-23
**Motiv:** deciziile de arhitectură luate pe o imagine incompletă riscă să fie greșite. Se așteaptă combinarea: audit cloud + audit local + istoric n8n/server + Control Plane.
**Consecință:** niciuna dintre DEC-1…DEC-4 nu se execută acum.

### D5 — Ordinea de execuție stabilită
**Cine:** Catalin · **Când:** 2026-08-23

**P0** — 1. acces comun ChatGPT ↔ Notion · 2. inventar local · 3. combinarea auditurilor · 4. Master Audit unic
**P1** — 5. `oracle_gateway` verificare și securizare · 6. backup ~170 workflow-uri n8n · 7. Render/servicii suspendabile · 8. Supabase pauzat
**P2** — 9. infrastructura de comunicare între agenți · 10. execuția automatizărilor

**Obiecție consemnată (Claude Code):** C1 și C3 au fost evaluate ca P0 în `04-RISK-REGISTER.md`. Un secret vizibil în sursa publică este o expunere activă, nu o constatare de audit — nu depinde de completitudinea imaginii, iar privatizarea val-ului durează un minut și e reversibilă. La fel, exportul workflow-urilor este citire pură. Decizia de ordonare aparține owner-ului și se respectă; obiecția rămâne consemnată pentru trasabilitate.

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
