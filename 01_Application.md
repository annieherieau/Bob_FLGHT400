
prompt : Explique-moi l'application contenue dans la bibliothèque FLGHT401

---

## Flight Reservation System (FRS) — Bibliothèque FLGHT401

### Vue d'ensemble

L'application **FLGHT401** est un **système de réservation de vols** (Flight Reservation System, abrégé *FRS*) développé sur IBM i (AS/400). Il s'agit d'une application 5250 (écrans verts) écrite en **RPG III / RPGLE** avec des fichiers **DDS** pour les écrans et les données. La bibliothèque contient à la fois le code source original (RPG III fixe) et des versions modernisées (ILE RPGLE free-format).

---

### Structure de navigation — les 3 menus

```
FRSMAIN — Menu principal
│
├── 1. Créer une réservation
├── 2. Mettre à jour une réservation existante
├── 3. Consulter une réservation
├── 4. Supprimer une réservation
├── 5. Envoyer la réservation par Fax
│
├── 10. Maintenance (→ menu FRSMANT)
│       ├── 1. Gestion des clients (Customer Master)
│       ├── 2. Gestion des horaires de vols (Flight Schedule)
│       ├── 3. Gestion du tableau des villes (City Table)
│       ├── 4. Gestion des compagnies aériennes (Airline Table)
│       └── 5. Gestion des agents (Agent Master)
│
└── 20. Rapports (→ menu FRSREPT)
        ├── 1. Rapport des commandes par agent — Tous agents (imprimé)
        ├── 2. Rapport des commandes par agent — Agent sélectionné (écran)
        ├── 3. Rapport des passagers par vol — Sélection (imprimé)
        └── 4. Rapport des passagers par vol — Sélection (écran)
```

---

### Modèle de données (fichiers physiques DDS)

| Fichier | Clé | Contenu |
|---|---|---|
| [`FLIGHTS.PF`](FLGHT401/QDDSSRCF/FLIGHTS.PF) | Numéro de vol | Vols : numéro, départ/arrivée (initiales + nom), jour de semaine, heures, compagnie, places disponibles, prix, kilométrage |
| [`CUSTOMERS.PF`](FLGHT401/QDDSSRCF/CUSTOMERS.PF) | Numéro client | Clients : numéro, nom |
| [`ORDERS.PF`](FLGHT401/QDDSSRCF/ORDERS.PF) | Numéro de commande | Réservations : n° commande, n° agent, n° client, n° vol, date de départ, nb billets, classe (1/2/3), signature requise |
| [`AGENTS.PF`](FLGHT401/QDDSSRCF/AGENTS.PF) | N° agent | Agents : numéro, nom, **mot de passe** |
| [`AIRLINE.PF`](FLGHT401/QDDSSRCF/AIRLINE.PF) | Nom compagnie | Compagnies aériennes : nom, initiales (3 caractères) |
| [`FRCITY.PF`](FLGHT401/QDDSSRCF/FRCITY.PF) | Ville de départ | Villes de départ : nom, initiales, compagnie, type d'appareil, numéro |
| [`TOCITY.PF`](FLGHT401/QDDSSRCF/TOCITY.PF) | Ville d'arrivée | Villes d'arrivée : nom, initiales, type d'appareil |

Des **fichiers logiques** (`.LF`) existent pour chaque fichier physique, permettant des accès triés par différents critères (nom, date, taille, etc.).

---

### Programmes principaux (QRPGSRC / QRPGLESRC)

| Programme | Rôle |
|---|---|
| [`FRS000.RPG`](FLGHT401/QRPGSRC/FRS000.RPG) | **Écran de connexion** — saisie nom d'agent + mot de passe, aiguillage vers CREATE / UPDATE / DSPLAY / DELETE / FAX / MAINT / REPORT |
| [`FRS001.RPG`](FLGHT401/QRPGSRC/FRS001.RPG) | **Création de réservation** — sélection vol, client, classe, date, nb billets ; écrit dans CUSTOMERS et ORDERS |
| [`FRS009.RPG`](FLGHT401/QRPGSRC/FRS009.RPG) | **Consultation / Modification / Suppression / Fax** d'une réservation |
| `FRS021.RPG` / [`FRS021`](FLGHT401/QRPGLESRC/FRS021) | **Gestion des horaires de vols** (maintenance) |
| `FRS401.RPG` / [`FRS401.RPGLE`](FLGHT401/QRPGLESRC/FRS401.RPGLE) | **Fenêtre de sélection d'agent** (sous-fichier paginé avec recherche) |
| `FRS402–FRS413` | Fenêtres de **sélection** (villes départ/arrivée, compagnies, vol complet, etc.) |
| `FRS403–FRS413` | Fenêtres popup pour la saisie assistée des champs |
| `FRS406.RPGLE` | Confirmation / envoi d'une réservation |
| `BCUST.RPG`, `BFLGHT.RPG` | Chargement initial (batch) des clients et vols |

---

### Programmes CLP utilitaires

| Programme | Rôle |
|---|---|
| [`FRS001CL.CLP`](FLGHT401/QCLPSRC/FRS001CL.CLP) | Wrapper pour `FRS001` : remplace le fichier CUSTOMER par `FLGHT400/CUSTOMERS` via `OVRDBF` |
| [`FRS411CL.CLP`](FLGHT401/QCLPSRC/FRS411CL.CLP) | Wrapper de maintenance |
| [`BLDCST.CLP`](FLGHT401/QCLPSRC/BLDCST.CLP) | Construction des coûts |
| [`RTSEED.CLP`](FLGHT401/QCLPSRC/RTSEED.CLP) | Réinitialisation des données de test (seed) |
| [`USERESET.CLP`](FLGHT401/QCLPSRC/USERESET.CLP) / [`USECLEAR.CLP`](FLGHT401/QCLPSRC/USECLEAR.CLP) | Commandes de remise à zéro |

---

### Flux principal — Création d'une réservation
<img width="200" alt="flux principal" src="https://github.com/user-attachments/assets/dd9e7c94-00f2-49c7-ab9f-7b53d89aedc2" />


### Aspect modernisation (en cours)

La bibliothèque contient deux générations de code en parallèle :

- **Ancienne génération** (`QRPGSRC/*.RPG`) : RPG III fixe, indicateurs numériques (`*IN50`, `*IN99`…), `EXSR`/`GOTO`, colonnes rigides.
- **Nouvelle génération** (`QRPGLESRC/*.RPGLE`) : ILE RPGLE `**FREE`, `DCL-F`/`DCL-S`/`DCL-DS`/`DCL-PROC`, indicateurs remplacés par des booléens nommés, zéro `GOTO`, appels API prototypés.

La documentation dans [`FRS401DOC.TXT`](FLGHT401/QRPGLESRC/FRS401DOC.TXT) et [`FRS021DOC`](FLGHT401/QRPGLESRC/FRS021DOC) détaille la stratégie de migration programme par programme, avec des listes de compilation et des grilles de tests.

---

### En résumé

**FLGHT401** est une application IBM i complète de gestion de réservations aériennes : connexion par agent, création/modification/suppression/consultation de réservations, tables de référence (vols, villes, compagnies, clients, agents), et édition de rapports. Elle est en cours de modernisation vers l'ILE RPGLE free-format tout en gardant les anciennes versions opérationnelles.
