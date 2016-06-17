---
title: Les nouveautés de Git 2.9
description: Il y a trois jours paraissait la version 2.9.0 de Git. Survol rapide des nouveautés.
url: /blog/metier/2016/les-nouveautes-de-git-2.9
creators: bma
---

_Cet article se base essentiellement sur la [publication de Github](https://github.com/blog/2188-git-2-9-has-been-released) annonçant la sortie de la nouvelle version. Pour une liste plus détaillée des fonctionnalités, voir [les notes de version](https://raw.githubusercontent.com/git/git/master/Documentation/RelNotes/2.9.0.txt)._

## Exécuter une commande pendant un rebase

Pendant un rebase interactif _(`git rebase -i`)_ il était déjà possible d'exécuter une commande entre deux commits en ajoutant une ligne avec le préfixe `x` ou `exec` puis la commande.

L'ajout de l'option `-x` permet maintenant d'exécuter une commande après **chaque** commit d'un rebase.

Cela ouvre plusieurs possibilités, comme lancer un _jeu de tests_ sur chaque commit pour corriger plusieurs défauts (là où un bisect devrait être utilisé pour chaque défaut), de compiler le code pour vérifier la syntaxe ou encore lancer du _linting_ et améliorer la qualité du code : le rebase est en pause si la commande utilisée renvoie une erreur. Un `git rebase --continue` reprend l'opération à tout moment.

```bash
git rebase -x 'make test'
```

## Des `diff` plus beaux

Cette nouvelle version améliore également le rendu des `diff` en rendant leur détection et coloration plus intelligentes _(par détection des blancs)_. Par exemple :
```diff
diff --git a/exemple.js b/exemple.js
index 7d021b1..1fac7e5 100644
--- a/exemple.js
+++ b/exemple.js
@@ -1,6 +1,10 @@
 function exemple (tab) {

     tab.forEach(element => {
+        action1(element);
+    });
+
+    tab.forEach(element => {
         action2(element);
     });
```
devient :
```diff
diff --git a/exemple.js b/exemple.js
index 7d021b1..1fac7e5 100644
--- a/exemple.js
+++ b/exemple.js
@@ -1,5 +1,9 @@
 function exemple (tab) {

+    tab.forEach(element => {
+        action1(element);
+    });
+
     tab.forEach(element => {
         action2(element);
     });
```
À activer avec l'option `--compaction-heuristic` ou la config `diff.compactionHeuristic`.

## Parallélisation

Depuis la version 2.8 de Git il était possible de paralléliser la récupération des sous-modules avec l'option `--jobs` :
```bash
git fetch --recurse-submodules
```
La 2.9 rend possible d'utiliser cette même option pour les `clone` et les `update` de sous-modules :
```bash
git clone --recurse-submodules --jobs=4 ...
git submodule update --jobs=4 ...
```
Et il est possible d'activer cette option par défaut :
```bash
git config submodule.fetchJobs X
```

## Et sinon…

La commande `git describe` a été améliorée pour fournir un descriptif de commit beaucoup plus pertinent.

Les tabulations sont mieux gérée dans l'affichage des `git log` _(C'est surtout interessant si vous avez l'habitude de faire de l'ascii art ou des tableaux en description de commit)_

Une nouvelle option `core.hooksPath` permet de choisir un autre dossier que `.git/hooks` pour chaque projet, et donc de pouvoir centraliser les hooks d'un ensemble de projet sans avoir à créer de liens symboliques.

---

Une erreur, une question, une remarque, n'hesitez pas : [@mab_](https://twitter.com/mab_) ou directement par une [pull-request](https://github.com/makinacorpus/blog-posts/blob/master/git-2.9.0.md).
