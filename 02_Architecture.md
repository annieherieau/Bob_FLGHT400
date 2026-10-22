=> **prompt 1/2** : fais-moi un diagramme d'architecture

<img width="3600" height="613" alt="archi" src="https://github.com/user-attachments/assets/ec6cb54a-431b-479a-8d87-88443ae80fe9" />

| Couleur | Couche | Rôle |
|---|---|---|
| 🔵 Bleu | Authentification | Point d'entrée unique — `FRS000` vérifie agent + mot de passe |
| 🟢 Vert | Menus DDS | Navigation hiérarchique : principal → maintenance → rapports |
| 🟡 Jaune | Réservations | CRUD complet sur les commandes de billets |
| 🟠 Orange | Maintenance | Administration des référentiels (vols, villes, compagnies, clients) |
| 🟣 Violet | Rapports | Éditions imprimées ou affichées à l'écran |
| 🔷 Cyan | Popups | Fenêtres de sélection assistée (sous-fichiers paginés) |
| ⚪ Gris | Base de données | Fichiers physiques + logiques DB2 for i |
| 🔴 Rouge | Batch/Utilitaires | Chargement initial et outils de réinitialisation |

Les **flèches pleines** représentent des appels de programmes (`CALL`), les **flèches pointillées** des accès en lecture/écriture aux fichiers DB2.

=> **prompt 2/2** : ce diagram est ilisible, propose-moi autre chose

[Diagramme d'architecture](https://github.com/annieherieau/Bob_FLGHT400/blob/main/architecture-flght401-flight-reservation-system.html)
