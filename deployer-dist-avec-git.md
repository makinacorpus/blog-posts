# Déployer séparément les fichiers css et js compilés avec git

Comment organiser un dépôt pour pouvoir travailler / merger / brancher sans les fichiers générés, tout en permettant le déploiement via Git de ces fichiers.

Sur un thème utilisant SASS, nous ne travaillons que sur les fichiers source `.scss`, et nous générons les fichiers `.css`.
Mais sur nos serveur de production, nous ne souhaitons pas installer ces outils.

Voici la procédure que nous suivons à Makina Corpus pour contourner ce problème.

Dans master, ignorer le dossier des fichiers compilés `.gitignore`

```sh
    /css
```

Créer une branche deploy

```sh
    git branch deploy
```

dans cette branch deploy, désignorer les fichiers compilés `.gitignore`

```sh
    #/css
```

récupérer les dernières versions des sources

```sh
    git fetch origin
    git merge master
```

Si on veut récupérer uniquement les sources (dans le cas où on les ignore dans deploy)

```sh
    git checkout master -- scss
```

On obtient dans deploy un commit de merge et un commit de build. Si on veut faire un seul commit on utilise l'option `--no-commit` : git s'arrête juste avant de faire le commit

*     lancer le build
*     ajouter les fichiers compilés
*     commiter
