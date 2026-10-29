## 📚 Librairie `FLGHT401`

### 🗄️ Fichiers de données — `QDDSSRCF` (Physical & Logical Files)

| Fichier | Type | Description probable |
|---|---|---|
| [`AIRLINE.PF`](FLGHT401/QDDSSRCF/AIRLINE.PF) | PF | Compagnies aériennes |
| [`AIRLINEL.LF`](FLGHT401/QDDSSRCF/AIRLINEL.LF) | LF | Accès logique sur AIRLINE |
| [`FLIGHTS.PF`](FLGHT401/QDDSSRCF/FLIGHTS.PF) | PF | Vols |
| [`FLIGHTSL.LF`](FLGHT401/QDDSSRCF/FLIGHTSL.LF) | LF | Accès logique sur FLIGHTS |
| [`FLIGHTSZ.LF`](FLGHT401/QDDSSRCF/FLIGHTSZ.LF) | LF | Accès logique sur FLIGHTS (alt) |
| [`CUSTOMERS.PF`](FLGHT401/QDDSSRCF/CUSTOMERS.PF) | PF | Clients |
| [`CUSTOMER.LF`](FLGHT401/QDDSSRCF/CUSTOMER.LF) | LF | Accès logique CUSTOMERS |
| [`CUSTNAME.LF`](FLGHT401/QDDSSRCF/CUSTNAME.LF) | LF | Clients par nom |
| [`CUSTOMRZ.LF`](FLGHT401/QDDSSRCF/CUSTOMRZ.LF) | LF | Clients (alt) |
| [`CUSTORD.LF`](FLGHT401/QDDSSRCF/CUSTORD.LF) | LF | Clients par commande |
| [`ORDERS.PF`](FLGHT401/QDDSSRCF/ORDERS.PF) | PF | Réservations/commandes |
| [`ORDERSZ.LF`](FLGHT401/QDDSSRCF/ORDERSZ.LF) | LF | Commandes (alt) |
| [`ORDDATE.LF`](FLGHT401/QDDSSRCF/ORDDATE.LF) | LF | Commandes par date |
| [`ORDNAME.LF`](FLGHT401/QDDSSRCF/ORDNAME.LF) | LF | Commandes par nom |
| [`AGENTS.PF`](FLGHT401/QDDSSRCF/AGENTS.PF) | PF | Agents |
| [`AGENTSL.LF`](FLGHT401/QDDSSRCF/AGENTSL.LF) | LF | Agents (logique) |
| [`AGENTSZ.LF`](FLGHT401/QDDSSRCF/AGENTSZ.LF) | LF | Agents (alt) |
| [`FRCITY.PF`](FLGHT401/QDDSSRCF/FRCITY.PF) | PF | Villes de départ (FR) |
| [`FRCITYL.LF`](FLGHT401/QDDSSRCF/FRCITYL.LF) | LF | Accès logique FRCITY |
| [`TOCITY.PF`](FLGHT401/QDDSSRCF/TOCITY.PF) | PF | Villes d'arrivée |
| [`TOCITYL.LF`](FLGHT401/QDDSSRCF/TOCITYL.LF) | LF | Accès logique TOCITY |
| [`FNAME.PF`](FLGHT401/QDDSSRCF/FNAME.PF) | PF | Prénoms |
| [`LNAME.PF`](FLGHT401/QDDSSRCF/LNAME.PF) | PF | Noms de famille |
| [`FNEXTO.PF`](FLGHT401/QDDSSRCF/FNEXTO.PF) | PF | Numéro séquentiel (prochain numéro commande ?) |

---

### 🖥️ Écrans — `QDDSSRCD` (Display Files)

| Fichier | Description probable |
|---|---|
| [`FRS000DF.DSPF`](FLGHT401/QDDSSRCD/FRS000DF.DSPF) à [`FRS413DF.DSPF`](FLGHT401/QDDSSRCD/FRS413DF.DSPF) | Écrans des programmes FRS000 → FRS413 |
| [`FRS401DFB.DSPF`](FLGHT401/QDDSSRCD/FRS401DFB.DSPF), [`FRS402DFB.DSPF`](FLGHT401/QDDSSRCD/FRS402DFB.DSPF) | Variantes B des écrans FRS401/402 |
| [`MSGPANEL.DSPF`](FLGHT401/QDDSSRCD/MSGPANEL.DSPF) | Panneau de messages générique |

---

### ⚙️ Programmes RPG ILE — `QRPGLESRC`

| Source | Description |
|---|---|
| [`FRS401.RPGLE`](FLGHT401/QRPGLESRC/FRS401.RPGLE) | Programme principal (ILE) |
| [`FRS401DOC.TXT`](FLGHT401/QRPGLESRC/FRS401DOC.TXT) | Documentation FRS401 |
| [`FRS402.RPGLE`](FLGHT401/QRPGLESRC/FRS402.RPGLE) / [`FRS402B.RPGLE`](FLGHT401/QRPGLESRC/FRS402B.RPGLE) | Programmes ILE FRS402 |
| [`FRS406.RPGLE`](FLGHT401/QRPGLESRC/FRS406.RPGLE) | Programme ILE FRS406 |
| [`FRS408.RPGLE`](FLGHT401/QRPGLESRC/FRS408.RPGLE) | Programme ILE FRS408 |
| [`FRS411.RPGLE`](FLGHT401/QRPGLESRC/FRS411.RPGLE) | Programme ILE FRS411 |
| [`FRS412.RPGLE`](FLGHT401/QRPGLESRC/FRS412.RPGLE) | Programme ILE FRS412 |
| [`FRS413.RPGLE`](FLGHT401/QRPGLESRC/FRS413.RPGLE) | Programme ILE FRS413 |
| [`FRS003.RPGLE`](FLGHT401/QRPGLESRC/FRS003.RPGLE) | Programme ILE FRS003 |
| [`FRS021`](FLGHT401/QRPGLESRC/FRS021) / [`FRS021DOC`](FLGHT401/QRPGLESRC/FRS021DOC) | Programme ILE FRS021 + doc |
| [`X2FRS024.RPGLE`](FLGHT401/QRPGLESRC/X2FRS024.RPGLE) / [`XXFRS024.RPGLE`](FLGHT401/QRPGLESRC/XXFRS024.RPGLE) | Variantes FRS024 (ILE) |

---

### ⚙️ Programmes RPG OPM — `QRPGSRC`

Série complète : `FRS000` → `FRS413` (versions OPM), plus des utilitaires :

| Source | Description |
|---|---|
| [`BFLGHT.RPG`](FLGHT401/QRPGSRC/BFLGHT.RPG) | Build/remplissage table FLIGHTS |
| [`BCUST.RPG`](FLGHT401/QRPGSRC/BCUST.RPG) | Build/remplissage table CUSTOMERS |
| [`RTRAND.RPG`](FLGHT401/QRPGSRC/RTRAND.RPG) | Routine aléatoire |
| [`RTDOFW.RPG`](FLGHT401/QRPGSRC/RTDOFW.RPG) | Routine jour de la semaine |
| [`GTOJCV.RPG`](FLGHT401/QRPGSRC/GTOJCV.RPG) / [`JTOGCV.RPG`](FLGHT401/QRPGSRC/JTOGCV.RPG) | Conversions Grégorien ↔ Julien |

---

### 🔧 Programmes CL — `QCLPSRC`

| Source | Description |
|---|---|
| [`FRS001CL.CLP`](FLGHT401/QCLPSRC/FRS001CL.CLP) | CL pour FRS001 |
| [`FRS411CL.CLP`](FLGHT401/QCLPSRC/FRS411CL.CLP) | CL pour FRS411 |
| [`BLDCST.CLP`](FLGHT401/QCLPSRC/BLDCST.CLP) | Construction des données clients |
| [`MSGPRG.CLP`](FLGHT401/QCLPSRC/MSGPRG.CLP) / [`MSGPRGH.CLP`](FLGHT401/QCLPSRC/MSGPRGH.CLP) / [`MSGPGM2.CLP`](FLGHT401/QCLPSRC/MSGPGM2.CLP) | Gestion des messages |
| [`RTSEED.CLP`](FLGHT401/QCLPSRC/RTSEED.CLP) | Initialisation graine aléatoire |
| [`TESTIT.CLP`](FLGHT401/QCLPSRC/TESTIT.CLP) | CL de test |
| [`USECLEAR.CLP`](FLGHT401/QCLPSRC/USECLEAR.CLP) / [`USERESET.CLP`](FLGHT401/QCLPSRC/USERESET.CLP) | Utilitaires reset/clear |

---

### 📋 Menus — `QMNUSRC`

| Source | Description |
|---|---|
| [`FRSMAIN.MNUDDS`](FLGHT401/QMNUSRC/FRSMAIN.MNUDDS) + [`FRSMAINQQ.MNUCMD`](FLGHT401/QMNUSRC/FRSMAINQQ.MNUCMD) | Menu principal |
| [`FRSMANT.MNUDDS`](FLGHT401/QMNUSRC/FRSMANT.MNUDDS) + [`FRSMANTQQ.MNUCMD`](FLGHT401/QMNUSRC/FRSMANTQQ.MNUCMD) | Menu maintenance |
| [`FRSREPT.MNUDDS`](FLGHT401/QMNUSRC/FRSREPT.MNUDDS) + [`FRSREPTQQ.MNUCMD`](FLGHT401/QMNUSRC/FRSREPTQQ.MNUCMD) | Menu rapports |

---

### 📊 Résumé

```
FLGHT401/
├── QDDSSRCF/   → 24 DDS (6 PF + 18 LF)
├── QDDSSRCD/   → 28 Display Files
├── QRPGLESRC/  → 14 sources RPG ILE
├── QRPGSRC/    → 37 sources RPG OPM
├── QCLPSRC/    → 10 sources CL
├── QCMDSRC/    → 2 commandes
└── QMNUSRC/    → 6 sources menus
```

Il s'agit d'une **application de réservation de vols** (Flight Reservation System — FRS), incluant la gestion des vols, compagnies aériennes, clients, agents et commandes/réservations, avec des versions OPM (RPG fixe) et ILE (RPGLE) coexistantes.

Voulez-vous approfondir un aspect particulier (diagramme ERD des fichiers, analyse d'un programme précis, comparaison OPM vs ILE, etc.) ?
