# EXPLICATIONS — A2 S2 CC Portail d'entraide

**Nom / Prénom :** _à compléter_

## Point de départ

- Commit initial de main : `4a266e1` (Créer le portail et le simulateur de créneaux).
- Commit de départ de main au début de l'épreuve : `76184a8` (Ajouter le conseil de vérification du compteur).
- `feature/tutorat` (fournie) : un commit `a43f314` posé sur `76184a8`.
- Identité vérifiée avec `git config user.name` / `git config user.email` avant mes commits.

## Branches

| Branche | Rôle | Point de départ |
|---|---|---|
| `feature/accueil` | Accueil libre sans inscription le jeudi à 16 h | `76184a8` |
| `feature/contact` | Adresse de contact cliquable (`mailto:`) | `76184a8` |
| `feature/tutorat` | Tutorat individuel sur inscription à 16 h 30 (fournie) | `76184a8` |
| `hotfix/duree` | Annulation de la durée erronée | `3b5cc66` (main après intégration) |

Les deux branches personnelles ont été créées depuis `76184a8` avant toute intégration (`git branch feature/accueil main`, `git branch feature/contact main`).

## Mes changements

- `f164b19` — feature/accueil : le paragraphe `#permanence` devient « Accueil libre sans inscription le jeudi à 16 h. »
- `e027d3b` — feature/contact : `entraide@example.org` est entouré d'un lien `<a href="mailto:entraide@example.org">`.
- Intégration dans main : `git merge feature/accueil` a fait une avance rapide (main pointait sur l'ancêtre direct), puis `git merge feature/contact` a créé le commit de fusion `8399342` (pas de conflit : lignes différentes).

## Conflit et décision de contenu

- **Pourquoi les branches ont divergé :** `feature/tutorat` et `feature/accueil` partent toutes deux de `76184a8` et modifient la même ligne (`<p id="permanence">`) de deux manières différentes. Git ne peut pas choisir seul.
- **État constaté :** `git merge feature/tutorat` → `CONFLICT (content): Merge conflict in index.html`, `git status` affiche `UU index.html` avec les marqueurs `<<<<<<<`, `=======`, `>>>>>>>`.
- **Décision :** garder les deux informations dans deux paragraphes distincts :
  - « Accueil libre sans inscription le jeudi à 16 h. » (`#permanence`)
  - « Tutorat individuel sur inscription le jeudi à 16 h 30. » (`#tutorat`)
  Le lien de contact (venu de `e027d3b`) n'était pas touché par le conflit et reste présent.
- **Vérification :** `grep -nE '^(<<<<<<<|=======|>>>>>>>)' index.html` ne renvoie rien ; `git add index.html` puis `git commit` → commit de fusion **`3b5cc66`**.
- **Parents de la fusion** (`git log --oneline --graph` ou `git show --format=%P 3b5cc66`) : `8399342` (main avec mes deux évolutions) et `a43f314` (feature/tutorat).

## Annulation ciblée

- Commit erroné : `b88cc98` — `app.js` : `dureeCreneau = 20` → `200`.
- Apport ultérieur à conserver : `76184a8` — section « Conseil de vérification » du README.
- Opération : `git switch -c hotfix/duree` depuis main (`3b5cc66`), puis `git revert b88cc98` → nouveau commit **`d136180`** qui remet `dureeCreneau = 20`. L'histoire n'est pas réécrite : `b88cc98` reste visible, suivi de son annulation.
- Pourquoi pas `git reset` / retour à une ancienne version : l'histoire est partagée, et revenir à `4a266e1` ferait perdre le conseil du README et toutes les contributions suivantes.
- Réunion dans main : `git merge hotfix/duree` (avance rapide, main = `d136180`).

## Tests

| Moment | Action | Attendu | Observé |
|---|---|---|---|
| Avant (main `76184a8`) | Ouverture | Durée affichée 200 (erreur connue) | 200 minutes par créneau |
| Avant | 2 clics « Ajouter » | 2 créneaux · 400 min | 2 créneau(x) · 400 minute(s) |
| Avant | « Recommencer » | 0 · 0 | 0 créneau(x) · 0 minute(s) |
| Après (main `d136180`) | Ouverture | 20 minutes par créneau | 20 |
| Après | 2 clics « Ajouter » | 2 créneaux · 40 min | 2 créneau(x) · 40 minute(s) |
| Après | « Recommencer » | 0 · 0 | 0 créneau(x) · 0 minute(s) |
| Après | Contenu | Accueil 16 h, tutorat 16 h 30, lien mailto, conseil README | Tous présents |

## Publication

- Dépôt public : https://github.com/Alexandre-bn/cc-portail-entraide
- Publication : `git push -u origin main feature/accueil feature/contact feature/tutorat hotfix/duree`.
- Vérification : `git rev-parse main` et `git ls-remote origin` donnent tous deux `6e37fe4` pour main après la première publication ; les quatre branches de tâche sont présentes sur GitHub.
- Le commit qui complète cette section est le dernier de main ; son identifiant, vérifié localement et sur GitHub, est noté dans le message de remise.

## Difficultés

Aucune autre difficulté que le conflit attendu décrit plus haut.

## Aides utilisées

- Claude Code (Claude Opus 5.5), utilisé comme corrigé d'entraînement : exécution des commandes Git, résolution du conflit, `git revert`, tests du simulateur et rédaction de ce document. Commits concernés : `f164b19`, `e027d3b`, `8399342`, `3b5cc66`, `d136180`.
- Vérification personnelle : _à compléter_.
- Ressources : documentation `git merge`, `git revert` (git-scm.com).
