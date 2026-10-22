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

[Diagramme d'architecture html](https://github.com/annieherieau/Bob_FLGHT400/blob/main/architecture-flght401-flight-reservation-system.html)

<img width="821" height="1094" alt="image" src="https://github.com/user-attachments/assets/9265bae0-182e-48a5-91c8-1f77c9b0be43" />
<img width="1065" height="895" alt="image" src="https://github.com/user-attachments/assets/95e8a70a-c357-4b37-aa67-3b07206b6f3a" />

