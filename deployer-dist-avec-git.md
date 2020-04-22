---
title: Déployer séparément les fichiers css et js compilés avec git
description: Comment organiser son dépôt pour travailler sans commiter les fichiers générés.
url: /blog/metier/2017/deployer-separement-les-fichiers-css-et-js-compiles-avec-git
---

Comment organiser un dépôt pour pouvoir travailler / merger / brancher sans les
fichiers générés, tout en permettant le déploiement via Git de ces fichiers.

Par exemple sur un thème utilisant SASS, nous ne travaillons que sur les
fichiers source `.scss`, et nous générons les fichiers `.css`. Lorsque nous
développons, nous préférons ne commiter que les fichiers source, et pas les
fichiers générés. Mais sur nos serveurs de production, nous ne souhaitons pas
installer les outils nous permettant de générer ces fichiers.

Pour résoudre cette quadrature du développement, il faut bien passer par Git.
L'idée est de **dédier une branche au déploiement de ces fichiers compilés**.
Voici la procédure que nous suivons à Makina Corpus pour contourner ce problème.

## Initialisation de la branche de déploiement.

Dans master, ignorer le dossier des fichiers compilés `.gitignore`

```bash
/css
```

Créer une branche `deploy`

```bash
(master)$ git co -b deploy
```

Dans cette branch `deploy`, désignorer les fichiers compilés `.gitignore`

```bash
#/css
```

Pousser la branche vers le dépôt distant

```bash
(deploy)$ git push origin deploy
```

## Mettre à jour la branche deploy

Récupérer la dernière version des sources depuis le dépôt distant.

```bash
(deploy)$ git pull origin/master
```

Les sources sont mergée dans la branche deploy.

Si on veut récupérer uniquement les sources (dans le cas où on les ignore dans
deploy)

```bash
(deploy)$ git checkout master -- scss
```

Générer les fichiers `js` et `css` avec votre outil préféré, puis les commiter.

```bash
(deploy)$ git add -p <vos fichiers>
(deploy)$ git commit
```

Pousser les commits vers le dépôt distant.

```bash
(deploy)$ git push origin deploy
```

On obtient dans deploy un commit de merge et un commit de build. Si on veut
faire un seul commit on utilise l'option `--no-commit` : git s'arrête juste
avant de faire le commit

- lancer le build
- ajouter les fichiers compilés
- commiter
