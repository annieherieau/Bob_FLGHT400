prompt : recherche tous les CALL FLGHT400 dans les sources, car il faudrait peut-être les reamplacer par FLGHT401

---
J'ai maintenant l'inventaire complet. Voici la synthèse structurée :

---

## Inventaire complet des références à `FLGHT400`

Il faut distinguer **3 catégories** qui n'ont pas toutes le même traitement :

---

### Catégorie 1 — `FRSMSGF FLGHT400` (fichier de messages)
> ⚠️ **À remplacer** si le fichier de messages `FRSMSGF` est dans `FLGHT401`, sinon **à laisser** si `FRSMSGF` reste dans `FLGHT400`.

C'est la catégorie la plus nombreuse — présente dans **presque tous les programmes** :

| Fichier | Ligne | Contenu |
|---|---|---|
| [`FRS000.RPG`](FLGHT401/QRPGSRC/FRS000.RPG:48) | 48 | `'FRSMSGF   FLGHT400  '` |
| [`FRS001.RPG`](FLGHT401/QRPGSRC/FRS001.RPG:71) | 71 | `'FRSMSGF   FLGHT400  '` |
| [`FRS001T.RPG`](FLGHT401/QRPGSRC/FRS001T.RPG:69) | 69 | idem |
| [`FRS001U.RPG`](FLGHT401/QRPGSRC/FRS001U.RPG:69) | 69 | idem |
| [`FRS001U2.RPG`](FLGHT401/QRPGSRC/FRS001U2.RPG:69) | 69 | idem |
| [`FRS001X.RPG`](FLGHT401/QRPGSRC/FRS001X.RPG:79) | 79 | idem |
| [`FRS002.RPG`](FLGHT401/QRPGSRC/FRS002.RPG:90) | 90 | idem |
| [`FRS005.RPG`](FLGHT401/QRPGSRC/FRS005.RPG:55) | 55 | idem |
| [`FRS009.RPG`](FLGHT401/QRPGSRC/FRS009.RPG:59) | 59 | idem |
| [`FRS021.RPG`](FLGHT401/QRPGSRC/FRS021.RPG:70) | 70 | idem |
| [`FRS022.RPG`](FLGHT401/QRPGSRC/FRS022.RPG:633) | 633 | idem |
| [`FRS023.RPG`](FLGHT401/QRPGSRC/FRS023.RPG:58) | 58 | idem |
| [`FRS024.RPG`](FLGHT401/QRPGSRC/FRS024.RPG:60) | 60 | idem |
| [`FRS401.RPG`](FLGHT401/QRPGSRC/FRS401.RPG:33) | 33 | idem |
| [`FRS401B.RPG`](FLGHT401/QRPGSRC/FRS401B.RPG:32) | 32 | idem |
| [`FRS402.RPG`](FLGHT401/QRPGSRC/FRS402.RPG:28) | 28 | idem |
| [`FRS402B.RPG`](FLGHT401/QRPGSRC/FRS402B.RPG:28) | 28 | idem |
| [`FRS403.RPG`](FLGHT401/QRPGSRC/FRS403.RPG:28) | 28 | idem |
| [`FRS404.RPG`](FLGHT401/QRPGSRC/FRS404.RPG:39) | 39 | idem |
| [`FRS404B.RPG`](FLGHT401/QRPGSRC/FRS404B.RPG:39) | 39 | idem |
| [`FRS405.RPG`](FLGHT401/QRPGSRC/FRS405.RPG:31) | 31 | idem |
| [`FRS407.RPG`](FLGHT401/QRPGSRC/FRS407.RPG:38) | 38 | idem |
| [`FRS408.RPG`](FLGHT401/QRPGSRC/FRS408.RPG:38) | 38 | idem |
| [`FRS412.RPG`](FLGHT401/QRPGSRC/FRS412.RPG:28) | 28 | idem |
| [`FRS413.RPG`](FLGHT401/QRPGSRC/FRS413.RPG:42) | 42 | idem |
| [`FRS402.RPGLE`](FLGHT401/QRPGLESRC/FRS402.RPGLE:52) | 52 | idem |
| [`FRS402B.RPGLE`](FLGHT401/QRPGLESRC/FRS402B.RPGLE:38) | 38 | idem |
| [`FRS408.RPGLE`](FLGHT401/QRPGLESRC/FRS408.RPGLE:66) | 66 | idem |
| [`FRS412.RPGLE`](FLGHT401/QRPGLESRC/FRS412.RPGLE:55) | 55 | idem |
| [`FRS413.RPGLE`](FLGHT401/QRPGLESRC/FRS413.RPGLE:85) | 85 | idem |
| [`FRS021`](FLGHT401/QRPGLESRC/FRS021:89) | 89 | idem |
| [`X2FRS024.RPGLE`](FLGHT401/QRPGLESRC/X2FRS024.RPGLE:84) | 84 | idem |
| [`XXFRS024.RPGLE`](FLGHT401/QRPGLESRC/XXFRS024.RPGLE:97) | 97 | idem |
| [`FRS024.RPGLE`](FLGHT401/EVFTEMPF01/FRS024.RPGLE:84) | 84 | idem |

---

### Catégorie 2 — Appels de programmes et menus qualifiés
> 🔴 **À remplacer par `FLGHT401`** si les programmes compilés sont dans `FLGHT401`.

| Fichier | Ligne | Contenu |
|---|---|---|
| [`FRS000.RPG`](FLGHT401/QRPGSRC/FRS000.RPG:72) | 72 | `'GO FLGHT400/FRSMANT'` |
| [`FRS000.RPG`](FLGHT401/QRPGSRC/FRS000.RPG:75) | 75 | `'GO FLGHT400/FRSREPT'` |
| [`FRSMANTQQ.MNUCMD`](FLGHT401/QMNUSRC/FRSMANTQQ.MNUCMD:2) | 2–6 | `CALL FLGHT400/FRS024`, `CALL FLGHT400/FRS021`, `UPDDTA FLGHT400/AIRLINE`, `CALL FLGHT400/FRS023` |
| [`FRS411CL.CLP`](FLGHT401/QCLPSRC/FRS411CL.CLP:3) | 3 | `DCLF FLGHT400/FRS411DF` |
| [`USERESET.CLP`](FLGHT401/QCLPSRC/USERESET.CLP:5) | 5, 8, 13 | `FLGHT400/FLDS` (data area) |
| [`USECLEAR.CLP`](FLGHT401/QCLPSRC/USECLEAR.CLP:5) | 5, 8, 13 | `FLGHT400/FLDS` (data area) |

---

### Catégorie 3 — Références DDS (REF / REFFLD / CHKMSGID)
> 🔴 **À remplacer** — les DDS référencent les fichiers physiques de `FLGHT400` pour hériter des descriptions de champs.

| Fichier | Objet référencé |
|---|---|
| [`FRS401DF.DSPF`](FLGHT401/QDDSSRCD/FRS401DF.DSPF) | `FLGHT400/CUSTNAME`, `FLGHT400/AGENTSL`, `FLGHT400/FRSMSGF` |
| [`FRS401DFB.DSPF`](FLGHT401/QDDSSRCD/FRS401DFB.DSPF) | `FLGHT400/AGENTSL` |
| [`FRS402DF.DSPF`](FLGHT401/QDDSSRCD/FRS402DF.DSPF) | `FLGHT400/FRCITY` |
| [`FRS402DFB.DSPF`](FLGHT401/QDDSSRCD/FRS402DFB.DSPF) | `FLGHT400/FRCITY` |
| [`FRS403DF.DSPF`](FLGHT401/QDDSSRCD/FRS403DF.DSPF) | `FLGHT400/TOCITY`, `FLGHT400/FRSMSGF` |
| [`FRS404DF.DSPF`](FLGHT401/QDDSSRCD/FRS404DF.DSPF) | `FLGHT400/FLIGHTSL`, `FLGHT400/FRSMSGF` |
| [`FRS408DF.DSPF`](FLGHT401/QDDSSRCD/FRS408DF.DSPF) | `FLGHT400/CUSTORD`, `FLGHT400/FRSMSGF` |
| [`FRS412DF.DSPF`](FLGHT401/QDDSSRCD/FRS412DF.DSPF) | `FLGHT400/FRSMSGF` |
| [`FRS413DF.DSPF`](FLGHT401/QDDSSRCD/FRS413DF.DSPF) | `FLGHT400/FLIGHTSZ`, `FLGHT400/FRSMSGF` |
| [`FRS024DF.DSPF`](FLGHT401/QDDSSRCD/FRS024DF.DSPF) | `FLGHT400/CUSTOMRZ` |

---

### Récapitulatif décisionnel

| Catégorie | Nb de fichiers | Action recommandée |
|---|---|---|
| Fichier de messages `FRSMSGF` | ~34 fichiers | Remplacer si `FRSMSGF` est copié dans `FLGHT401`, sinon laisser |
| `CALL`/`GO`/`DCLF` vers programmes | 3 fichiers (+ MNUCMD) | **Remplacer par `FLGHT401`** |
| `REF`/`REFFLD`/`CHKMSGID` dans DDS | 10 fichiers | **Remplacer par `FLGHT401`** (nécessite recompilation des display files) |

Voulez-vous que je procède aux remplacements ?
