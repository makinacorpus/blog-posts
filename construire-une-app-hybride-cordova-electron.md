# Construire une application hybride mobile et desktop avec Cordova et Electron

On connait maintenant bien les [applications mobiles hybrides](http://makina-corpus.com/blog/metier/2016/quelle-solution-pour-mon-application-mobile-hybride), mais il est également possible de créer une application desktop grâce [Electron](http://electron.atom.io/) à partir de langages web.

![](http://makina-corpus.com/blog/metier/2016/module-desktop-mobile)

## Qu'est ce qu'Electron ?

Electron est un outil créer par les équipes de Github qui permet d'encapsuler une web app pour en faire **un exécutable sur desktop** ou une application installable via un exécutable (compatible Windows, Mac OS, Linux). Les éditeurs de texte Atom et Visual Studio Code sont  par exemple fait à partir d'Electron.

Le principe est donc à peu près le même que pour Cordova, notre app tourne dans une webview basée sur Chromium.


### Quelques différences techniques avec Cordova

* Electron intègre de base BabelJS et permet très facilement de coder en ES2015
* Les [Web API](https://developer.mozilla.org/fr/docs/Web/API) sont en général mieux supportées, de nombreuses fonctionnalités peuvent donc être utilisées directement avec la Web API comme pour du web classique, sans passer par un quelquonque plugin (exemple : Talk To Speech, Camera, Vibrate, etc)
* Pas besoin de compiler à chaque modification, un simple `ctrl + R` suffit à mettre à jour l'application


## Construire une app modulaire

En construisant notre app sous forme de modules, il est ensuite très simple de réutiliser son code pour différents projets.
Nous allons créer ici 3 dossiers : **electron**, **cordova** et **modules**.

Les dossiers cordova et electron contiennent tous les deux uniquement une base fraichement installée (`cordova create cordova` pour cordova et le [quick start pour electron](https://github.com/electron/electron-quick-start) seront de bonnes bases). Tous nos développement (ou presque) se feront dans nos modules.

Le rendu HTML sera uniquement généré en JS pour éviter d'avoir à écrire ou à importer du HTML dans chacun des projets. J'utilise ici React pour simplifier la manipulation du DOM.


### Comment gérer les besoins spécifiques (plugins Cordova, etc) ?

Comme évoqué précédemment, nous avons parfois besoin d'implémenter des plugins à Cordova pour accéder à certaines fonctionnalités, tandis qu'Electron gère très bien la Web API.

Nous utiliserons donc des modules pour chacunes des fonctionnalités nécessitant un plugin.

NB : Il est aussi possible d'utiliser des modules pour concevoir un comportement différent selon le type de device.


![](http://makina-corpus.com/blog/metier/2016/arborescence-app-cordova-electron)

Dans notre exemple, nous utilisons la fonctionnalité TextToSpeech (tts), qui fonctionne avec la Web API pour Electron, mais qui nécéssite un plugin pour Cordova. Nous avons donc **2 modules spécifiques** (se terminant respectivement par .webapi ou .cordova) et **un module principal** (reader) qui sera le coeur de notre projet.


### Comment utiliser nos modules ?

#### **./modules/reader/reader.js** (notre module principal)
Le coeur de notre application se trouve dans `./modules/reader`. Nous lui passons en paramètres ce qui doit être spécifique à une plateforme.
Elle prend ici en options un objet contenant :

* **container** : l'élément HTML dans lequel nous voulons initialiser notre app
* **tts** : notre fonction de TTS (car Cordova pass par un plugin et devra avoir une syntaxe légèrement différente)
* **text** : Le texte que nous voulons lire

Comme vous l'aurez remarqué, la syntaxe utilisée est en ES2015, il faudra donc au préalable utiliser BabelJS et un module loader (Webpack, Browserify,...) pour l'utilisation dans Cordova.

<p data-height="523" data-theme-id="dark" data-slug-hash="MebXYQ" data-default-tab="js" data-user="lellex" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/lellex/pen/MebXYQ/">Reader module</a> by Alexandra J (<a href="http://codepen.io/lellex">@lellex</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

<br>


#### **./modules/tts.webapi/tts.webapi.js** (service spécifique pour electron ou web)
`./modules/tts.webapi` est notre fonction de TTS pour Electron (ou pour le web puisqu'il s'agit de la [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API))

<p data-height="420" data-theme-id="dark" data-slug-hash="WxoJKx" data-default-tab="js" data-user="lellex" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/lellex/pen/WxoJKx/">TTS Web API</a> by Alexandra J (<a href="http://codepen.io/lellex">@lellex</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

<br>


#### **./modules/tts.cordova/tts.cordova.js** (service spécifique pour cordova)

Pour utiliser la synthèse vocale dans Cordova, nous allons avoir besoin de [ce plugin](https://github.com/vilic/cordova-plugin-tts) car la Web API ne fonctionnera pas. Il faudra donc installer le plugin dans notre projet cordova et créer une fonction correspondante dans nos modules (la syntaxe à l'utilisation n'est pas tout à fait la même que le web).

<p data-height="255" data-theme-id="dark" data-slug-hash="wWoQYN" data-default-tab="js" data-user="lellex" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/lellex/pen/wWoQYN/">TTS Cordova</a> by Alexandra J (<a href="http://codepen.io/lellex">@lellex</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

<br>

#### **./electron**
Le chargement des modules peut se faire via npm, en pointant vers son adresse s'il est en ligne ou notre dossier local (pas besoin de `.js` si notre module a un fichier `main` référencé dans son `package.json`)

```
npm install ./../modules/reader
```
Une fois notre package installé, nous pouvons les utiliser avec un `require` ou `import` dans Electron.
Il suffira ensuite d'initialiser notre fonction de lecture avec les bons paramètres (la fonction TTS ainsi que la phrase que nous voulons lire).

<p data-height="233" data-theme-id="dark" data-slug-hash="ezBrKG" data-default-tab="js,result" data-user="lellex" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/lellex/pen/ezBrKG/">Reader</a> by Alexandra J (<a href="http://codepen.io/lellex">@lellex</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

<br>

#### **./cordova/www/index.js**

Pour Cordova, nous installons les modules de la même manière, avec npm. Par contre il n'y a pas de module loader contrairement à Electron, nous devrons donc charger les fichiers dans `index.html`.

Notre fichier JS cordova fait appel à Reader de la même manière qu'Electron, avec des options différentes.

<p data-height="400" data-theme-id="dark" data-slug-hash="RRoqzB" data-default-tab="js" data-user="lellex" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/lellex/pen/RRoqzB/">Cordova</a> by Alexandra J (<a href="http://codepen.io/lellex">@lellex</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

<br>

### [Démo](https://github.com/lellex/demo-electron-cordova)

[Une version démo](https://github.com/lellex/demo-electron-cordova) de l'exemple ci-dessus est disponible sur Github avec des explications plus détaillées.
Chacun des modules a ici été mis sur un dépot, ce qui nous permet d'utiliser directement npm pour les installer.


## Les application desktop ne sont donc pas mortes ?

Electron apporte aux développeurs web la possibilité de compiler leurs applications en une version desktop de manière très simple. Néanmoins, les applications packagées tendent à être de moins en moins utilisées avec [l'évolution des navigateurs et des usages du web](http://makina-corpus.com/blog/metier/2016/introduction-progressive-web-apps).

Comme pour le développement mobile, on peut se poser la question du choix de la techno : **progressive web app** ou **webview** ? Dans tous les cas, si notre application métier est conçue comme un module réutilisable, il sera assez aisé de changer d'environnement.
