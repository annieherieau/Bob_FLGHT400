
Prompt : lorsque je fais option 1. create order puis F4 FROM cities j'ai l'erreur suivante : L'erreur RPG0202 a entraîné l'arrêt du programme FRS000.Recherche et explique le problème

---

## Cause de l'erreur RPG0202 dans FRS000

### RPG0202 — définition

L'erreur **RPG0202** sur IBM i signifie :

> **"Called program or procedure not found."**
> Un `CALL` a été exécuté vers un programme qui n'existe pas dans la liste de bibliothèques.

---

### Le chemin d'appel fautif

Voici exactement ce qui se passe quand vous faites Opt 1 → F4 :

```
FRS000  (RUNTYP='CREATE')
  └── CALL 'FRS001'          ← ligne 227 de FRS000.RPG
        └── EXSR @F04SR      ← ligne 523 de FRS001.RPG (F4 pressé)
              └── CALL 'FRS402'   ← ligne 526 de FRS001.RPG
                    ↑
              ⚠️ Programme introuvable → RPG0202 remonté jusqu'à FRS000
```

### Où est le problème

[`FRS001.RPG`](FLGHT401/QRPGSRC/FRS001.RPG:526) appelle `FRS402` avec **3 paramètres** :
```rpg
C                     CALL 'FRS402'
C                     PARM           FCITY             * ville (16A)
C                     PARM           FCITYI            * initiales (3A)
C                     PARM           FCITYA            * compagnie aérienne (3A)
```

Et [`FRS402.RPGLE`](FLGHT401/QRPGLESRC/FRS402.RPGLE:18) déclare bien ces 3 paramètres :
```rpgle
DCL-PI *N;
  FRMCTY CHAR(16);
  FRMINT CHAR(3);
  FRMAIR CHAR(3);
END-PI;
```

Le code source est cohérent — **mais l'erreur RPG0202 signifie que l'objet programme `FRS402` n'est tout simplement pas compilé (ou pas présent) dans la liste de bibliothèques au moment de l'exécution.**

---

### Les causes possibles

| Cause | Explication |
|---|---|
| **`FRS402.RPGLE` n'a jamais été compilé** | Le source existe dans `QRPGLESRC` mais aucun objet `*PGM` n'a été créé dans la bibliothèque |
| **Compilé dans la mauvaise bibliothèque** | L'objet `FRS402` existe dans `FLGHT401` mais la liste de bibliothèques pointe sur `FLGHT400` (c'est le nom cité dans les `OVRDBF` et `CALL` CLP) |
| **Objet supprimé ou non migré** | L'ancienne version RPG III `FRS402.RPG` n'existe pas dans `QRPGSRC` — il n'y a que la version RPGLE modernisée |

La 2e cause est la plus probable : les programmes CL comme [`FRS001CL.CLP`](FLGHT401/QCLPSRC/FRS001CL.CLP) font référence à **`FLGHT400`** (pas `FLGHT401`), ce qui indique que la bibliothèque d'exécution attendue est `FLGHT400` :

```clp
OVRDBF FILE(CUSTOMER) TOFILE(FLGHT400/CUSTOMERS)
CALL   FRS001 PARM(&AGENT)
```

La documentation de modernisation ([`FRS401DOC.TXT`](FLGHT401/QRPGLESRC/FRS401DOC.TXT)) parle aussi de `FLGHT400` pour les compilations.

---

### Solution

**1. Compiler `FRS402.RPGLE`** dans la bibliothèque correcte (`FLGHT400` ou celle dans votre liste de bibliothèques) :

```cl
CRTBNDRPG PGM(FLGHT400/FRS402)
          SRCFILE(FLGHT401/QRPGLESRC)
          SRCMBR(FRS402)
          DFTACTGRP(*NO)
          ACTGRP(*NEW)
          DBGVIEW(*LIST)
```

**2. Vérifier que tous les programmes appelés par `FRS001` sont compilés** — notamment `FRS403` (F5), `FRS413` (F6), `FRS406` (confirmation), car le même problème se produira en appuyant sur ces touches.

**3. Vérifier votre liste de bibliothèques** avec `DSPLIBL` : `FLGHT400` (ou la bibliothèque cible) doit y figurer pour que les `CALL` sans qualification de bibliothèque aboutissent.
