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

### D5 — Ordinea de execuție (revizuită)
**Cine:** Catalin · **Când:** 2026-08-23 · **Versiune:** 2

| Prioritate | Acțiune | Motiv |
|---|---|---|
| **P0** | Securizare `oracle_gateway` | Secret expus public = risc activ |
| **P0** | Backup/export workflow-uri n8n | Citire/export, risc mic de schimbare, pierdere potențială mare |
| **P1** | Master Audit complet | Trebuie să știm exact ce avem |
| **P1** | Rezolvarea accesului comun Notion | Necesită colaborarea reală Claude ↔ ChatGPT |
| **P1** | Render / plăți | Prevenirea întreruperii infrastructurii |
| **P2** | DEC-1…DEC-4 | Decizii de arhitectură, după audit |

**Istoric:** versiunea 1 plasa cele două acțiuni de securitate în P1, după Master Audit. Claude Code a obiectat: o expunere activă nu depinde de completitudinea imaginii, iar ambele acțiuni sunt reversibile sau read-only. Obiecția a fost acceptată de owner și prioritizarea a fost corectată. Separarea reținută: **acțiunile de audit** nu așteaptă auditul; **deciziile de arhitectură** îl așteaptă.

### D6 — Cele două P0 sunt RECOMANDATE, nu executate
**Cine:** Catalin · **Când:** 2026-08-23
**Regulă:** identificarea unei acțiuni ca P0 nu constituie execuția ei. `oracle_gateway` rămâne public și niciun workflow n8n nu a fost exportat. Ambele sunt înregistrate ca acțiuni prioritare pentru etapa de execuție, care nu a început.
**Stare la această dată:** C1 NEREZOLVAT · C3 NEREZOLVAT.

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
