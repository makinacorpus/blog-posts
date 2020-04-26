---
url: /blog/metier/2015/bien-demarrer-avec-ionic
---

# Bien démarrer avec Ionic

## Historique

Ionic va vous permettre de développer une application web qui aura l'air d'une application native. Elle en partagera également certaines capacités comme l'accès aux éléments systèmes. C'est ce qu'on appelle une application hybride. La majeure partie de son code est en technologie du web (javascript/html/css) et pour tout le reste, il y a des plugins qui font l'interface entre le smartphone et votre application.

[Ionic 1](http://ionicframework.com/) est basé sur [Cordova](https://cordova.apache.org/) et [Angular 1](https://angularjs.org/). Il fournit des briques qui vous permettront de créer une application proche du natif. [Les équipes de Drifty](http://drifty.com/) ont mis tout en œuvre pour faciliter la vie du développeur mobile hybride en lui fournissant un environnement simple et un framework pratique. Leur framework a tellement fait sensation qu'ils ont [levé un million de dollars](http://techcrunch.com/2014/03/10/drifty-makers-of-the-ionic-mobile-framework-raise-1-million/) en 2014, alors que la startup n'avait que 2 ans d’existence. Cela a permis à leurs équipes de repenser leur création, en apprenant de leurs erreurs, des demandes de la communauté et en mettant à jour les briques sur lesquelles reposent le framework. [Ionic 2](http://ionic.io/2) est notamment passé de Angular 1 à [Angular 2](https://angular.io/) et utilise désormais [webpack](https://webpack.github.io/) en lieu et place de bower et gulp. Ionic 2 permet également d'utiliser [les évolutions de javascript](https://twitter.com/BrendanEich/status/415303474591436801) avec les syntaxes d'ES6/ES2015 et d'ES7. [Une présentation a été faite à l'AngularConnect](https://www.youtube.com/watch?v=bAlydPwFONY) qui [a fait bonne impression](http://makina-corpus.com/blog/metier/2015/les-nouveautes-dionic-2-retours-de-langularconnect) tant les performances ont été améliorées, surtout concernant les animations.


## Tutoriel

Il est temps de rentrer dans le vif du sujet et de créer notre première application Ionic !

### Installer Ionic
Commençons par installer notre environnement de travail.
- Installez [node.js](https://nodejs.org/en/) ;
- Et les outils en ligne de commande (CLI) pour cordova et Ionic :

```
$ npm install -g ionic cordova
```

### Créer un projet Ionic
Pour commencer une application avec Ionic, il suffit d'utiliser la CLI. Essayons la commande suivante :

```
$ ionic templates
```

Cette commande liste une partie des templates officiels disponibles. Sachez qu'il en existe plusieurs [trouvables notamment sur Github](https://github.com/search?utf8=%E2%9C%93&q=starter-ionic).

Pour avoir un bon aperçu de ce que propose le framework, nous allons partir d'un template assez complet :

```
$ ionic start myApp tabs
```

À ce stade, nous avons un répertoire contenant le patron minimum de notre future application.

```
$ tree myApp -L 2
.
├── bower.json
├── config.xml
├── gulpfile.js
├── hooks
│   ├── after_prepare
│   └── README.md
├── ionic.project
├── package.json
├── plugins
│   ├── cordova-plugin-console
│   ├── cordova-plugin-device
│   ├── cordova-plugin-splashscreen
│   ├── cordova-plugin-statusbar
│   ├── cordova-plugin-whitelist
│   ├── fetch.json
│   └── ionic-plugin-keyboard
├── scss
│   └── ionic.app.scss
└── www
    ├── css
    ├── img
    ├── index.html
    ├── js
    ├── lib
    └── templates
```

Si vous avez déjà fait des projets web, vous devriez retrouver vos petits :

- des fichiers de configuration pour les outils : _bower.json_, _gulpfile.js_, _package.json_ ;
- des fichiers de configuration pour cordova : _config.xml_, _ionic.project_.

Nous reviendrons plus tard sur les répertoires `hooks` et `plugins`. Il ne reste donc plus qu'à voir les fichiers sources contenus dans les répertoires `scss` et surtout `www`.

C'est dans ce dernier que nous allons développer l'application web qui deviendra grâce à Ionic et Cordova une application hybride pour Android ou iOS. Pour en avoir un aperçu, après tout, cela reste du web, nous pouvons lancer la commande suviante depuis le répertoire `myApp` :

```
$ cd myApp
$ ionic serve -l
```

Une page dans notre navigateur va s'ouvrir pour présenter les deux versions dans des cadres dédiés à chaque plateforme (`-l` correspondant à l'option _lab_, qui vous permet d'observer les 2 versions simultanément).

Et voilà ! Nous pouvons d'ores et déjà intéragir avec notre nouvelle application. L'un des très grands avantages de cette fonctionnalité est que la page se met à jour automatiquement, grâce à livereload, lorsque nous effectuons des changements dans nos templates, nos scripts ou nos feuilles de style.

### Ajouter des plugins avec Ionic
Revenons au répertoire `plugins` ; ce dernier stocke, vous l'aurez deviné, les plugins de notre application. Ces éléments vont nous permettre une interaction entre le smartphone et l'application web. Celle-ci tourne dans une _webview_ et les plugins seront le lien entre cette webview et les API des différents outils disponibles dans le SDK du smartphone de vos utilisateurs. Lançons donc `ionic plugin` pour découvrir la liste des plugins déjà installés :

```
$ ionic plugin
cordova-plugin-console 1.0.1 "Console"
cordova-plugin-device 1.0.1 "Device"
cordova-plugin-splashscreen 2.1.0 "Splashscreen"
cordova-plugin-statusbar 1.0.1 "StatusBar"
cordova-plugin-whitelist 1.0.0 "Whitelist"
ionic-plugin-keyboard 1.0.8 "Keyboard"
```

En prenant le dernier élément comme exemple, `ionic-plugin-keyboard`, voici ce que dit le _README_ qui se trouve dans `./plugins/ionic-plugin-keyboard/README.md` :

> The `cordova.plugins.Keyboard` object provides functions to make interacting with the keyboard easier, and fires events to indicate that the keyboard will hide/show.

> _L'objet `cordova.plugins.Keyboard` fournit des fonctions permettant une interaction plus facile avec le clavier, et émet des événements pour indiquer que le clavier est affiché/caché._

Concernant le `cordova-plugin-whitelist`, voici ce qu'explique son fichier `README.md` :

> This plugin implements a whitelist policy for navigating the application webview on Cordova 4.0.
>
> _Ce plugin implémente une politique de liste blanche pour la navigation dans la webview sous Cordova 4.0._

Voilà donc le principe. Les plugins nous permettent d'accéder à ce qui est géré en dehors de la webview. Et si nous avons besoin d'accéder à l'appareil photo du smartphone, nous pouvons utiliser la commande :

```
$ ionic plugin add cordova-plugin-camera
```

### Développer l'application
Nous pouvons désormais écrire et améliorer notre application. Ionic utilise AngularJS comme base principale ce qui fait qu'il est quasi indispensable de s'en servir. Mais, pas de panique, ce n'est pas si compliqué de faire des choses simples avec ; si vous découvrez le framework, vous apprendrez plein de choses assez rapidement, et apprendre c'est bien !

De plus, les développeurs du ce merveilleux outil nous ont préparé une liste importantes de [composants](http://ionicframework.com/docs/api/) et d'[éléments](http://ionicframework.com/docs/components/) prêts à l'emploi et documentés !

Il y a déjà de quoi créer facilement la base de votre application, et c'est plus au niveau de la customisation qu'il vous faudra passer du temps.
Enfin, il est important de noter que la gestion de la navigation et de l'arborescence est gérée par la bibliothèque [UI-Router](https://github.com/angular-ui/ui-router), une extension très prisée de tous les développeurs AngularJS.

### Déployer une application Ionic
Que serait une application pour smartphone si l'on ne la fait pas tourner dans un smartphone ? Ici nous entrons dans le monde de Cordova, mais comme Ionic est un bel outil, il reprend la plupart des commandes pour ne pas avoir à se poser la question : est-ce que c'est du Ionic ou du Cordova ?

Ici et comme sur les smartphones, il y a deux mondes : Android et iOS. Pour chacun de ces deux mondes, il faut installer les outils de développement spécifiques à chacun avant de pouvoir émuler ou compiler l'application.

Pour installer les outils iOS, il faut être dans la sphère Apple et donc posséder une machine avec OSX et XCode.

Pour [installer les outils Android](https://developer.android.com/sdk/installing/index.html?pkg=tools), c'est un peu plus ouvert car basé sur java, et nous avons donc la possibilité d'utiliser au choix, Windows, OSX ou une distribution Linux.

Une fois l'environnement prêt, nous pouvons générer les fichiers nécessaires à la plateforme désirée :

```
$ ionic platform add android
$ ionic platform add ios
```

Vous pouvez alors packager votre application pour la tester soit sur un émulateur, soit sur un périphérique réel. De manière générale il est toujours préférable de tester sur de vrais smartphones et tablettes, notamment à cause de la  lenteur et l'imperfection des émulateurs fournis.

Pour packager :

```
$ ionic build android
$ ionic build ios
```

Pour émuler :

```
$ ionic emulate android
$ ionic emulate ios
```

Pour déployer sur un périphérique de test, cela diffère selon la plateforme :

Pour **Android** :
```
$ ionic run android
```

_Cette commande lancera la version debug de notre application sur le premier périphérique trouvé, et si aucun n'est trouvé sur un émulateur._

Pour **iOS**, vous avez deux solutions :
- Ouvrir le fichier XCode généré dans le dossier `platforms/ios` avec XCode lui-même, puis sélectionner le périphérique souhaité via l'interface et cliquer sur le bouton `Play`.
- Installer le packet npm [ios-deploy](https://www.npmjs.com/package/ios-deploy) en global afin de pouvoir lancer directement l'application sur le périphérique via la commande :
```
    ionic run ios --device
```

### Tester une application Ionic
Il est important de noter que l'APK généré par le packaging Android peut être envoyé à n'importe qui pour être testé sur son propre appareil mobile. Cet APK se trouve dans `platforms/android/build/outputs/apk/android-debug.apk`
La version iOS quand à elle ne peut être partagée facilement. Il est nécessaire de préparer une version beta via _iTunes Connect_ et d'y inscrire des comptes Apple enregistrés auprès des machines de tests.


## Astuces Ionic

### AngularJS
AngularJS propose différentes façons de faire la même chose. Il n'est jamais simple de s'y retrouver. C'est pourquoi nous vous recommandons de faire preuve de régularité dans votre code. [John Papa](https://twitter.com/john_papa) (développeur expert chez Google) et [Todd Motto](https://twitter.com/toddmotto) (co-fondateur de voux.io) ont longtemps discuté des bonnes pratiques et des patterns à appliquer lorsqu'on code avec AngularJS avant d'en proposer [des recommandations](https://github.com/johnpapa/angular-styleguide) et [autres règles à suivre](https://github.com/toddmotto/angularjs-styleguide).

### Feuilles de style
Ionic est basé en grande partie sur l'utilisation de Sass, et rend notamment beaucoup de choses paramétrables via des variables. Pour cette raison, et bien d'autres (notamment l'organisation des classes css), nous vous recommandons d'[activer dès le départ Sass](http://ionicframework.com/docs/cli/sass.html) sur votre projet via la commande :

```
$ ionic setup sass
```

### Communauté
Outre la documentation officielle, il existe une vaste communauté prompte à contribuer au monde Ionic. Vous trouverez beaucoup de [ressources sur Github](https://github.com/search?utf8=%E2%9C%93&q=ionic). [Andrew McGivery](https://twitter.com/andrewmcgivery) propose un [article recensant plus de 230 ressources ionic](http://mcgivery.com/100-ionic-framework-resources/) (_une sorte d'[awesome](https://github.com/sindresorhus/awesome)-ionic_)!

### Plugins
L'utilisation de [ngCordova](http://ngcordova.com/) est fortement conseillée. Il s'agit d'un ensemble de plugins choisis et maintenus par l'équipe d'Ionic, et donc toujours compatibles avec le framework.

### Symboles et icônes
Ionic embarque de base [Ionicons](http://ionicons.com/), une banque d'icônes très complète avec des déclinaisons par plateforme. N'oubliez pas de l'utiliser ! C'est le [font-awesome](https://fortawesome.github.io/Font-Awesome/) d'Ionic.

### Splashscreens et icônes d'application
Ionic offre également [un service](http://ionicframework.com/docs/cli/icon-splashscreen.html) permettant de générer très rapidement tous les formats d'icônes et de splashscreens nécessaires au déploiement sur les deux plateformes. Pour cela ils vous suffit de placer deux images, une pour l'icône et une pour le splashscreen, dans le répertoire `resources`. Les images peuvent être des `.png`, `.psd` ou `.ai` et doivent être au minimum 192×192 px pour l'icône de lancement et 2208×2208 px avec l'image centrée. [Un template](http://code.ionicframework.com/resources/splash.psd) et plus d'informations sont disponibles dans [un article de Drifty](http://blog.ionic.io/automating-icons-and-splash-screens/). Enfin pour générer les ressources :
```
$ ionic resources
```

_Attention cependant, l'outil ne permet pas encore la génération de format 9-patch pour les splashscreens Android._

### Compatibilité
La compatibilité avec les vieux périphériques Android peut être un vrai problème. À ce problème Ionic nous propose d'utiliser [Crosswalk](https://crosswalk-project.org/), et d'[embarquer au sein de l'application une version récente de Chrome](http://blog.ionic.io/crosswalk-comes-to-ionic/) afin d'assurer un rendu et des performances optimales. Attention cependant, cela n'est utile que si votre but et de toucher les anciennes versions d'Android. Car cet ajout a un coup non négligeable : il ajoute plusieurs dizaines de Mo à votre application. Pour se faire, il suffit d'entrer la commande :

```
$ ionic browser add crosswalk
```

### Tests
Afin de faciliter ce processus, les développeurs nous ont gratifié de l'application [Ionic View](http://view.ionic.io/) (disponible sur le [Play Store](https://play.google.com/store/apps/details?id=com.ionic.viewapp) et sur l'[App Store](https://itunes.apple.com/us/app/ionic-view/id849930087?ls=1&mt=8)) qui permet d'envoyer et de gérer des versions de tests sur plusieurs périphériques. Cela nous permet de simplement partager l'application quelque soit la pateforme où l'on désire tester.
