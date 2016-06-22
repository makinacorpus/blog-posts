# Construire une application hybride mobile et desktop avec Cordova et Electron

## Qu'est ce qu'Electron ?

On connait maintenant bien les applications hybrides en mode modile utilisant Cordova, mais il est également possible de faire de l'hybride desktop via [Electron](http://electron.atom.io/). Electron est un outil créer par les équipes de Github qui permet d'encapsuler une app web pour en faire **une app installable sur desktop** (compatible Windows, Mac OS, Linux). Les éditeurs de texte Atom et Visual Studio Code sont des app Electron.

Le principe est donc à peu près le même que pour Cordova, on a une app web qui tourne dans une webview basée sur Chromium.


### Quelques différences avec Cordova

* Electron intègre de base BabelJS et permet très facilement de coder en ES2015
* Les [Web API](https://developer.mozilla.org/fr/docs/Web/API) sont en général mieux supportées, de nombreuses fonctionnalités peuvent donc être utilisées directement avec la Web API sans passer par un quelquonque plugin (exemple : Talk To Speech, Camera, Vibrate, etc)
* Pas besoin de compiler à chaque modification, un simple `ctrl + R` suffit à reloader l'application


## Construire une app modulaire

En construisant notre app sous forme de modules, il est ensuite très simple de réutiliser son code pour différents projets.
Nous allons créer ici **un projet Electron** et **un projet Cordova** qui utiliserons chacun **un module commun** + des modules spécifiques selon leur besoin.

[image arborescence]

*Dans mon application, le dossier modules contient mon module commun **der-reader**, ainsi que plusieurs modules spécifiques (.cordova ou .webapi)*

Le dossier Cordova et le dossier Electron contiennent tous les deux uniquement une base fraichement installée. Tous nos développement (ou presque) se feront dans des modules spécifiques.

Le rendu HTML sera donc uniquement généré en JS, j'utilise ici React pour simplifier la manipulation du DOM.
