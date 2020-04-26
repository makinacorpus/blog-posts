# Retour d'expérience avec le framework Vue.js

[Vue](https://vuejs.org/) est un framework javascript crée par Evan You en 2013. Il a été conçu pour réaliser des interfaces utilisateurs de manière simple et efficace. Pour la première fois, nous avons pu le tester sur un projet chez Makina.

## Comment ça marche ?

### Initialisation

Pour commencer, vue peut s'installer avec la plupart des méthodes : CLI, NPM, ou simplement à l'aide d'une balise `<script>` et un CDN.

Nous sommes partis du CLI car c'est ce qui nous semble le plus souple pour le choix des outils. Contrairement à angular-cli ou create-react-app, [**vue-cli**](https://github.com/vuejs/vue-cli) utilise un système de **templates**. Un template est une "base" permettant d'initialliser un projet avec selon le template choisi, des outils de test, lint, build, hot reload, etc. Il éxiste 5 templates officiels : webpack, webpack-simple, browserify, browserify-simple ou simple.

```bash
# install vue-cli
$ npm install --global vue-cli
# create a new project using the "webpack" template
$ vue init webpack my-project
# install dependencies and go!
$ cd my-project
$ npm install
$ npm run dev
```

### Écosystème

De nombreuses librairies sont disponible pour Vue, le repo [awesome-vue](https://github.com/vuejs/awesome-vue) les référence par thème / popularité.

Pour notre projet, nous avons utilisé **vue-router** pour le routing, **vuex** pour architecturer les données (équivalent à redux) et [**element**](http://element.eleme.io/) pour les composants graphiques.


## Les tests

Le template que nous avons choisi (webpack) inclu les librairies de test **Karma / Mocha / Chai**. En ayant l'habitude des tests avec React et Jest, la configuration de base paraît un peu frustrante : contrairement à Jest, les diffs de codes n'apparaissent pas aussi clairement, ainsi que les numéro de ligne.
Il semble également y avoir un soucis de configuration des **sourcemap javascript** avec ce template, ce qui **ne facilite pas vraiment le debug**.


## Points forts
* Rapide à prendre en main
* Configuration simplifiée

## Points négatifs
* Écosystème majoritairement sinophone, peut être un frein pour contruibuer ou chercher de l'aide sur certaines librairies 
* Manque de ressources (peu de références sur Stackoverflow / Github)
