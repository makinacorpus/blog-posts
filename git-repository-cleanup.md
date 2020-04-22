---
title: Nettoyer un dépôt Git
description: Comment nettoyer en profondeur un dépôt Git
url: /blog/metier/2016/nettoyer-un-depot-git
---

Pour diverses raisons (erreurs, mauvaise gestion, …),
il arrive qu'un dépôt git prenne une taille démesurée,
ce qui peu vite devenir handicapant.
Il devient donc parfois nécessaire de faire un peu de ménage
et retirer complètement certains fichiers du dépôt
peut être une solution efficace.

Attention, lorsqu'on dit ici retirer un fichier du dépôt,
il ne s'agit pas de simplement faire un commit le supprimant
(ce qui ne changerait en rien la taille du dépôt),
mais de modifier l'historique complet des commits
pour faire en sorte de l'en retirer complètement.
Cette ré-écriture complète de l'historique rendra le dépôt
**incompatible** avec tout autre clone du même projet.

_Cette solution peu également permettre de retirer d'un projet
des fichiers contenant des données sensibles ou confidentielles._

### À partir d'un dépôt local, à jour :

* Retirer tous les remotes
```bash
git remote rm origin
```
* Supprimer toutes les références du fichier à effacer :
```bash
git filter-branch --index-filter 'git rm --cached --ignore-unmatch fichier_a_effacer.zip -- --all'
```
>   * `filter-branch` est la commande permettant de ré-écrire la branche courante.
>     * `--index-filter` permet d'accélérer le traitement en traitant l'index au lieu des fichiers du disque.
>   * `rm` supprime.
>     * `--cached` modifie l'index et la zone d'attente au lieu des fichiers du disque.
>     * `--ignore-unmatch` permet de ne pas générer d'erreur si la référence à supprimer n'existe pas.
>     * `-- --all` permet d'agir sur toutes les branches.

* La commande `filter-branch` génère automatiquement une sauvegarde qu'il nous faut supprimer pour réellement alléger le dépôt _(**Attention**, à partir d'ici on est réellement destructif, pas de retour en arrière possible)_ :
```bash
rm -rf .git/refs/original/
rm -rf .git/logs/
```

* Pour terminer, le _garbage collector_ va supprimer tous les objets qui ne sont plus référencés :
```bash
git gc --aggressive --prune=now
```
>   * `--aggressive` optimise plus profondément le dépôt.
>   * `--prune=now` supprime tous les objets inutilisés immédiatement (par défaut, `--prune` supprime uniquement les objets inutilisés depuis plus de deux semaines).


Pour une version plus détaillée, vous pouvez vous référer à la [version française du livre <u>Pro Git</u>](http://git-scm.com/book/fr/v2/Les-tripes-de-Git-Maintenance-et-r%C3%A9cup%C3%A9ration-de-donn%C3%A9es#Suppression-d’objets) accessible librement en ligne.
