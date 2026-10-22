
prompt : que fait le programme **FRS409** ?

---

## Ce que fait FRS409

**FRS409** est un programme RPG (RPG/400) qui affiche une **fenêtre de confirmation de billet** (*Ticket Confirmation Window*).

### Fonctionnement détaillé

1. **Paramètres d'entrée/sortie**
   - `ORDER#` (9 chiffres) : le numéro de billet/commande concerné
   - `ENDYN` (1 caractère) : indicateur retourné au programme appelant (`'Y'` = quitter, `'N'` = continuer)

2. **Affichage**
   Le programme affiche une **fenêtre popup** (15 lignes × 29 colonnes, positionnée en ligne 5 colonne 2) contenant le message :
   > *"Ticket Number **[ORDER#]** has been modified on the order file. To make additional flight modifications press ENTER. Otherwise, press F3 to exit from Ticket Order Maintenance."*

3. **Touches disponibles**
   - **ENTRÉE** → retourne `ENDYN = 'N'` au programme appelant (l'utilisateur veut faire d'autres modifications de vols)
   - **F3** ou **F12** → retourne `ENDYN = 'Y'` au programme appelant (l'utilisateur veut quitter la maintenance des billets)

### Résumé

FRS409 est une **fenêtre de confirmation intermédiaire** affichée après qu'un billet de transport aérien a été modifié dans le fichier des commandes. Elle demande à l'utilisateur s'il souhaite continuer à modifier d'autres vols (ENTRÉE) ou s'il veut quitter le module de maintenance des billets (F3/F12). Le résultat est retourné via le paramètre `ENDYN` au programme appelant.
