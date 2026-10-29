# Architecture Overview — FLGHT401 (Flight Reservation System)

## 1. Description Générale

**FLGHT401** est un système de réservation de vols interactif (*Flight Reservation System — FRS*) développé pour IBM i. Il s'exécute dans la bibliothèque **FLGHT400** (bibliothèque cible de production) et repose sur des programmes RPG OPM (RPG III), ILE RPGLE (format fixe et libre), ainsi que des programmes CL. L'application couvre le cycle complet de la réservation : authentification d'un agent, création / modification / suppression / consultation / envoi par fax d'une réservation, ainsi que la maintenance des données de référence (vols, villes, compagnies aériennes, clients, agents). Des rapports (queries) complètent l'offre fonctionnelle.

La source se trouve dans la bibliothèque source **FLGHT401**, organisée en plusieurs membres :

| Répertoire source | Contenu |
|---|---|
| [`QRPGSRC`](FLGHT401/QRPGSRC) | Programmes OPM RPG III (production historique) |
| [`QRPGLESRC`](FLGHT401/QRPGLESRC) | Programmes ILE RPGLE (modernisés, format libre) |
| [`QCLPSRC`](FLGHT401/QCLPSRC) | Programmes CL |
| [`QDDSSRCD`](FLGHT401/QDDSSRCD) | Fichiers d'écrans DDS (display files) |
| [`QDDSSRCF`](FLGHT401/QDDSSRCF) | Fichiers de base de données DDS (PF/LF) |
| [`QMNUSRC`](FLGHT401/QMNUSRC) | Menus DDS (FRSMAIN, FRSMANT, FRSREPT) |
| [`QCMDSRC`](FLGHT401/QCMDSRC) | Commandes IBM i (USERESET, USECLEAR) |

---

## 2. Flux Principaux

### 2.1 Authentification de l'agent

```
FRSMAIN (menu principal)
  └─► FRS000 (logon / sélection du type d'opération)
         ├─ Valide agent + mot de passe via AGENTSL
         └─ F4 ─► FRS401 (sélection d'agent par sous-fichier)
```

L'agent saisit son nom et son mot de passe dans [`FRS000.RPG`](FLGHT401/QRPGSRC/FRS000.RPG). Le programme reçoit un paramètre `RUNTYP` (6 car.) qui indique l'action demandée : `CREATE`, `UPDATE`, `DSPLAY`, `DELETE`, `FAX`, `MAINT` ou `REPORT`. Après validation, il route vers le programme approprié.

### 2.2 Flux de création d'une réservation (CREATE)

```
FRS000 (RUNTYP='CREATE')
  └─► FRS001 (saisie réservation principale)
         ├─► FRS402 / FRS403  (sélection ville départ / arrivée)
         ├─► FRS413           (sélection de vol)
         ├─► FRS405           (sélection client)
         ├─► FRS411           (envoi FAX confirmation)
         └─ Écrit dans ORDERS / FNEXTO (numéro de commande suivant)
```

### 2.3 Flux de mise à jour / consultation / suppression

```
FRS000 (RUNTYP='UPDATE'|'DSPLAY'|'DELETE'|'FAX')
  └─► FRS009 (sélection de commande)
         ├─► FRS002 (modification réservation)  ─► sous-sélecteurs FRS4xx
         ├─► FRS003 (consultation réservation)
         ├─► FRS004 (suppression réservation)
         ├─► FRS005 (fax réservation)           ─► FRS411
         ├─► FRS406 (confirmation Y/N)
         ├─► FRS407 (liste commandes par date)
         └─► FRS408 (liste commandes par client)
```

### 2.4 Flux Maintenance (MAINT)

```
FRSMAIN → option 10 → GO FRSMANT (menu maintenance)
  ├─ 1 ─► FRS024 (maintenance clients)
  ├─ 2 ─► FRS021 (maintenance horaires de vol)
  ├─ 3 ─► FRS022 (maintenance villes / compagnies)
  ├─ 4 ─► UPDDTA AIRLINE (maintenance directe airline table)
  └─ 5 ─► FRS023 (maintenance agents)
```

### 2.5 Flux Rapports (REPORT)

```
FRSMAIN → option 20 → GO FRSREPT (menu rapports)
  ├─ 1 ─► SBMJOB RUNQRY ALLAGENTS   (toutes commandes par agent — impression)
  ├─ 2 ─► RUNQRY SELAGENTS          (commandes agent sélectionné — écran)
  ├─ 3 ─► RUNQRY PRTFLIGHT          (passagers par vol — impression)
  └─ 4 ─► RUNQRY DSPFLIGHT          (passagers par vol — écran)
```

---

## 3. Programmes Clés et leurs Rôles

### 3.1 Point d'entrée et routage

| Programme | Type | Rôle |
|---|---|---|
| [`FRS000.RPG`](FLGHT401/QRPGSRC/FRS000.RPG) | OPM RPG III | Écran de logon + routage principal selon `RUNTYP`. Authentifie l'agent contre AGENTSL. Appelle `QCMDEXC` pour naviguer vers les menus FRSMANT/FRSREPT. |
| [`FRS001CL.CLP`](FLGHT401/QCLPSRC/FRS001CL.CLP) | CL | Wrapper CL : applique `OVRDBF FILE(CUSTOMER)` puis appelle `FRS001` avec le numéro d'agent. |

### 3.2 Gestion des réservations

| Programme | Type | Rôle |
|---|---|---|
| [`FRS001.RPG`](FLGHT401/QRPGSRC/FRS001.RPG) | OPM RPG III | Saisie/création d'une réservation. Accède à CUSTOMER, CUSTNAME, ORDERS, FLIGHTSZ, FRCITY, TOCITY. |
| [`FRS001U.RPG`](FLGHT401/QRPGSRC/FRS001U.RPG) | OPM RPG III | Variante mise à jour (UPDATE) — accède également à FNEXTO et ORDERS en output. |
| [`FRS001T.RPG`](FLGHT401/QRPGSRC/FRS001T.RPG) | OPM RPG III | Variante temporaire / test. |
| [`FRS001U2.RPG`](FLGHT401/QRPGSRC/FRS001U2.RPG) | OPM RPG III | Deuxième variante mise à jour. |
| [`FRS001X.RPG`](FLGHT401/QRPGSRC/FRS001X.RPG) | OPM RPG III | Variante consultation — accède à ORDERS en entrée (renommage de record format). |
| [`FRS002.RPG`](FLGHT401/QRPGSRC/FRS002.RPG) | OPM RPG III | Modification d'une commande existante (ORDNAME, ORDERSZ, FLIGHTSZ). |
| [`FRS003.RPG`](FLGHT401/QRPGSRC/FRS003.RPG) / [`FRS003.RPGLE`](FLGHT401/QRPGLESRC/FRS003.RPGLE) | OPM + ILE | Consultation d'une commande. Accède à ORDNAME et FLIGHTSZ. |
| [`FRS004.RPG`](FLGHT401/QRPGSRC/FRS004.RPG) | OPM RPG III | Suppression d'une commande (ORDERSZ en UF, ORDNAME, FLIGHTSZ). |
| [`FRS005.RPG`](FLGHT401/QRPGSRC/FRS005.RPG) | OPM RPG III | Envoi FAX d'une commande (ORDNAME, FLIGHTSZ). |
| [`FRS009.RPG`](FLGHT401/QRPGSRC/FRS009.RPG) | ILE free-form | Sélection de commande — liste les ordres et permet de choisir l'action (UPDATE/DSPLAY/DELETE/FAX). Accède à FORDERSZ. |

### 3.3 Sous-sélecteurs (fenêtres popup)

| Programme | Type | Rôle |
|---|---|---|
| [`FRS401.RPG`](FLGHT401/QRPGSRC/FRS401.RPG) / [`FRS401.RPGLE`](FLGHT401/QRPGLESRC/FRS401.RPGLE) | OPM + ILE modernisé | Sous-fichier sélection d'agent (AGENTSL). Retourne nom agent. |
| [`FRS401B.RPG`](FLGHT401/QRPGSRC/FRS401B.RPG) | OPM RPG III | Variante ancienne du sélecteur agent. |
| [`FRS402.RPG`](FLGHT401/QRPGSRC/FRS402.RPG) / [`FRS402.RPGLE`](FLGHT401/QRPGLESRC/FRS402.RPGLE) | OPM + ILE | Sous-fichier sélection ville départ (FRCITY). Retourne nom ville, code international, code compagnie. |
| [`FRS402B.RPGLE`](FLGHT401/QRPGLESRC/FRS402B.RPGLE) | ILE free-form | Variante sans repositionnement du sélecteur ville départ. |
| [`FRS403.RPG`](FLGHT401/QRPGSRC/FRS403.RPG) | OPM RPG III | Sous-fichier sélection ville d'arrivée (TOCITY). |
| [`FRS404.RPG`](FLGHT401/QRPGSRC/FRS404.RPG) | OPM RPG III | Sous-fichier sélection de vol (FLIGHTSL). |
| [`FRS405.RPG`](FLGHT401/QRPGSRC/FRS405.RPG) | OPM RPG III | Sous-fichier sélection client (CUSTNAME). |
| [`FRS406.RPGLE`](FLGHT401/QRPGLESRC/FRS406.RPGLE) | ILE (fixe/mixte) | Fenêtre de confirmation Y/N (reçoit ORDER#, retourne ENDYN). |
| [`FRS407.RPG`](FLGHT401/QRPGSRC/FRS407.RPG) | OPM RPG III | Sous-fichier liste commandes par date (ORDDATE). |
| [`FRS408.RPG`](FLGHT401/QRPGSRC/FRS408.RPG) / [`FRS408.RPGLE`](FLGHT401/QRPGLESRC/FRS408.RPGLE) | OPM + ILE | Sous-fichier liste commandes par client (CUSTORD). |
| [`FRS412.RPGLE`](FLGHT401/QRPGLESRC/FRS412.RPGLE) | ILE free-form | Sous-fichier sélection de compagnie aérienne (AIRLINE). Retourne nom + code. |
| [`FRS413.RPGLE`](FLGHT401/QRPGLESRC/FRS413.RPGLE) | ILE free-form | Sous-fichier sélection de vol (FLIGHTSZ). Retourne 14 champs de vol. |

### 3.4 Maintenance des données de référence

| Programme | Type | Rôle |
|---|---|---|
| [`FRS021.RPG`](FLGHT401/QRPGSRC/FRS021.RPG) | OPM RPG III | Maintenance des horaires de vol (FLIGHTSZ en UF, AIRLINE, FRCITY, TOCITY). |
| [`FRS022.RPG`](FLGHT401/QRPGSRC/FRS022.RPG) | ILE free-form | Maintenance villes/compagnies aériennes avec SQL embarqué. |
| [`FRS023.RPG`](FLGHT401/QRPGSRC/FRS023.RPG) | OPM RPG III | Maintenance agents (AGENTSZ en UF, AGENTSL). |
| [`FRS024.RPG`](FLGHT401/QRPGSRC/FRS024.RPG) | OPM RPG III | Maintenance clients (CUSTOMRZ en UF, CUSTNAME). |
| [`X2FRS024.RPGLE`](FLGHT401/QRPGLESRC/X2FRS024.RPGLE) / [`XXFRS024.RPGLE`](FLGHT401/QRPGLESRC/XXFRS024.RPGLE) | ILE free-form | Versions modernisées de FRS024 (avec SQL pour X2, RLA pour XX). |
| [`FRS411.RPGLE`](FLGHT401/QRPGLESRC/FRS411.RPGLE) | ILE free-form | Fenêtre FAX simulée avec délais progressifs (écran d'animation). |
| [`FRS411CL.CLP`](FLGHT401/QCLPSRC/FRS411CL.CLP) | CL | Version CL du FAX — enchaîne plusieurs SNDRCVF avec DLYJOB. |

### 3.5 Utilitaires et chargement de données

| Programme | Type | Rôle |
|---|---|---|
| [`BFLGHT.RPG`](FLGHT401/QRPGSRC/BFLGHT.RPG) | OPM RPG III | Chargement batch des vols (lit FRCITY/TOCITY, génère FLIGHTS). Utilise des tables internes pour heures/jours/prix. |
| [`BCUST.RPG`](FLGHT401/QRPGSRC/BCUST.RPG) | OPM RPG III | Chargement batch des clients (tire noms aléatoires depuis FNAME/LNAME, écrit CUSTOMER). |
| [`BLDCST.CLP`](FLGHT401/QCLPSRC/BLDCST.CLP) | CL | Wrapper pour BCUST : applique OVRDBF sur CUSTOMER. |
| [`RTRAND.RPG`](FLGHT401/QRPGSRC/RTRAND.RPG) | OPM RPG III | Sous-routine : génère un nombre aléatoire entre deux bornes. |
| [`RTDOFW.RPG`](FLGHT401/QRPGSRC/RTDOFW.RPG) | OPM RPG III | Retourne le jour de la semaine. |
| [`GTOJCV.RPG`](FLGHT401/QRPGSRC/GTOJCV.RPG) / [`JTOGCV.RPG`](FLGHT401/QRPGSRC/JTOGCV.RPG) | OPM RPG III | Conversions date Grégorienne ↔ Julienne. |
| [`MSGPRG.CLP`](FLGHT401/QCLPSRC/MSGPRG.CLP) | CL | Boucle d'attente (25 000 itérations) — simulation de traitement. |
| [`USERESET.CLP`](FLGHT401/QCLPSRC/USERESET.CLP) / [`USECLEAR.CLP`](FLGHT401/QCLPSRC/USECLEAR.CLP) | CL | Commandes utilitaires (reset / purge de données utilisateur). |

---

## 4. Diagramme d'Architecture Mermaid

...

---

## 5. Tables de Base de Données

### Fichiers physiques (Physical Files — PF)

| Fichier | Record Format | Clé | Description |
|---|---|---|---|
| [`AGENTS.PF`](FLGHT401/QDDSSRCF/AGENTS.PF) | `ORDERS` *(sic)* | `AGENT_NO` (9B) | Table des agents : numéro, nom (64A varlen), mot de passe (64A varlen) |
| [`CUSTOMERS.PF`](FLGHT401/QDDSSRCF/CUSTOMERS.PF) | `CUSTOMERS` | `CUSTO00001` (9B) → alias `CUSTOMER_NO` | Clients : numéro, nom (64A varlen) |
| [`FLIGHTS.PF`](FLGHT401/QDDSSRCF/FLIGHTS.PF) | `FLIGHTS` | `FLIGH00001` (9B) → alias `FLIGHT_NUMBER` | Vols : N° vol, départ, arrivée, jour semaine, heure départ/arrivée, compagnie, sièges dispo, tarif, mileage |
| [`ORDERS.PF`](FLGHT401/QDDSSRCF/ORDERS.PF) | `ORDERS` | `ORDER00001` (9B) → alias `ORDER_NUMBER` | Commandes : N° commande, N° agent, N° client, N° vol, date départ, nb billets, classe, signature |
| [`AIRLINE.PF`](FLGHT401/QDDSSRCF/AIRLINE.PF) | `AIRLNR` | `AIRLNM` (16A) | Compagnies aériennes : nom + code initiales (3A) |
| [`FRCITY.PF`](FLGHT401/QDDSSRCF/FRCITY.PF) | `FRCTYR` | `FRCNAM` (16A) | Villes de départ : nom, initiales, compagnie, type d'appareil, numéro de ville |
| [`TOCITY.PF`](FLGHT401/QDDSSRCF/TOCITY.PF) | `TOCTYR` | `TOCNAM` (16A) | Villes d'arrivée : nom, initiales, type d'appareil |
| [`FNEXTO.PF`](FLGHT401/QDDSSRCF/FNEXTO.PF) | — | — | Séquenceur de numéro de commande suivant |
| `FNAME.PF` / `LNAME.PF` | — | — | Tables de prénoms / noms pour génération batch de clients |

### Fichiers logiques (Logical Files — LF)

| Fichier | Basé sur | Rôle |
|---|---|---|
| [`AGENTSL.LF`](FLGHT401/QDDSSRCF/AGENTSL.LF) | AGENTS | Tri par nom agent — utilisé par les sélecteurs (FRS401, FRS023) |
| [`AGENTSZ.LF`](FLGHT401/QDDSSRCF/AGENTSZ.LF) | AGENTS | Accès direct (clé N° agent) — mise à jour |
| [`CUSTOMER.LF`](FLGHT401/QDDSSRCF/CUSTOMER.LF) | CUSTOMERS | Accès direct par N° client |
| [`CUSTOMRZ.LF`](FLGHT401/QDDSSRCF/CUSTOMRZ.LF) | CUSTOMERS | Accès mise à jour par N° client |
| [`CUSTNAME.LF`](FLGHT401/QDDSSRCF/CUSTNAME.LF) | CUSTOMERS | Tri par nom client — sélecteur FRS405 |
| [`CUSTORD.LF`](FLGHT401/QDDSSRCF/CUSTORD.LF) | ORDERS + CUSTOMERS | Vue jointe commandes + nom client — sélecteur FRS408 |
| [`FLIGHTSL.LF`](FLGHT401/QDDSSRCF/FLIGHTSL.LF) | FLIGHTS | Tri par ville départ — sélecteur FRS404 |
| [`FLIGHTSZ.LF`](FLGHT401/QDDSSRCF/FLIGHTSZ.LF) | FLIGHTS | Accès direct par N° vol — sélecteur FRS413, maintenance FRS021 |
| [`ORDNAME.LF`](FLGHT401/QDDSSRCF/ORDNAME.LF) | ORDERS + CUSTOMERS | Vue jointe commandes + nom client — FRS002/003/005 |
| [`ORDERSZ.LF`](FLGHT401/QDDSSRCF/ORDERSZ.LF) | ORDERS | Accès direct (update/delete) — FRS002/004/009 |
| [`ORDDATE.LF`](FLGHT401/QDDSSRCF/ORDDATE.LF) | ORDERS | Tri par date de départ — FRS407 |
| [`AIRLINEL.LF`](FLGHT401/QDDSSRCF/AIRLINEL.LF) | AIRLINE | Tri par nom — FRS021 |
| [`FRCITYL.LF`](FLGHT401/QDDSSRCF/FRCITYL.LF) | FRCITY | Tri par nom ville départ — FRS022 |
| [`TOCITYL.LF`](FLGHT401/QDDSSRCF/TOCITYL.LF) | TOCITY | Tri par nom ville arrivée |

---

## 6. Stratégie de Modernisation

Le projet est en cours de migration progressive OPM → ILE. Le fichier [`FRS401DOC.TXT`](FLGHT401/QRPGLESRC/FRS401DOC.TXT) documente en détail la démarche adoptée :

| Axe | OPM (ancien) | ILE modernisé |
|---|---|---|
| Format | RPG III colonnes fixes | `**FREE` — `dcl-*` |
| Indicateurs | `*IN40`, `*IN42`… | Variables booléennes nommées via `INDDS` |
| Subroutines | `BEGSR/ENDSR` | Procédures `dcl-proc` |
| Appels API | `CALL/PARM` | Prototypes `dcl-pr extpgm` |
| Structures de données | I-specs | `dcl-ds qualified` |
| Accès données | RLA (READ/SETLL/SETGT) | SQL embarqué (FRS022) ou RLA nommé |
| Flux | `GOTO`, `CASxx` | `SELECT/WHEN`, `IF/ENDIF`, `RETURN` |

Plusieurs programmes coexistent en double version (ex. [`FRS401.RPG`](FLGHT401/QRPGSRC/FRS401.RPG) OPM et [`FRS401.RPGLE`](FLGHT401/QRPGLESRC/FRS401.RPGLE) ILE) pour permettre les tests côte-à-côte avant bascule en production.

---

## 7. Résumé des Dépendances Clés

```
FRS000 ──────────────────────── AGENTSL (lecture auth)
  ├── FRS001 ──────────────────── CUSTOMER, CUSTNAME, ORDERS, FNEXTO, FLIGHTSZ
  │     ├── FRS402 ──────────── FRCITY
  │     ├── FRS403 ──────────── TOCITY
  │     ├── FRS413 ──────────── FLIGHTSZ
  │     ├── FRS405 ──────────── CUSTNAME
  │     └── FRS411 ──────────── (display file uniquement)
  └── FRS009 ──────────────────── ORDERSZ
        ├── FRS002 ──────────── ORDNAME, ORDERSZ, FLIGHTSZ
        ├── FRS003 ──────────── ORDNAME, FLIGHTSZ
        ├── FRS004 ──────────── ORDNAME, ORDERSZ
        └── FRS005 ──────────── ORDNAME, FLIGHTSZ → FRS411
```
