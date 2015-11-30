# Bien démarrer avec Ionic

## Historique

Ionic va vous permettre de développer une application web qui aura l'air d'une application native. Elle en partagera également certaines capacités comme l'accès aux éléments systèmes. C'est ce qu'on appelle une application hybride. La majeure partie de son code est en technologie du web (javascript/html/css) et pour tout le reste, il y a des plugins qui font l'interface entre le smartphone et votre application.

[Ionic 1](http://ionicframework.com/) est basé sur [Cordova](https://cordova.apache.org/) et [Angular 1](https://angularjs.org/). Il fournit des briques qui vous permettront de créer une application proche du natif. [Les équipes de Drifty](http://drifty.com/) ont mis tout en œuvre pour faciliter la vie du développeur mobile hybride en lui fournissant un environnement simple et un framework pratique. Leur framework a tellement fait sensation qu'ils ont [levé un million de dollars](http://techcrunch.com/2014/03/10/drifty-makers-of-the-ionic-mobile-framework-raise-1-million/) en 2014, alors que la startup n'avait que 2 ans d’existence. Cela a permis à leurs équipes de repenser leur création, en apprenant de leurs erreurs, des demandes de la communauté et en mettant à jour les briques sur lesquelles reposent le framework. [Ionic 2](http://ionic.io/2) est notamment passé de Angular 1 à [Angular 2](https://angular.io/) et utilise désormais [webpack](https://webpack.github.io/) en lieu et place de bower et gulp. Ionic 2 permet également d'utiliser [les évolutions de javascript](https://twitter.com/BrendanEich/status/415303474591436801) avec les syntaxes d'ES6/ES2015 et d'ES7. [Une présentation a été faite à l'AngularConnect](https://www.youtube.com/watch?v=bAlydPwFONY) qui [a fait bonne impression](http://makina-corpus.com/blog/metier/2015/les-nouveautes-dionic-2-retours-de-langularconnect) tant les performances ont été améliorées, surtout concernant les animations.


## Tutoriel

Il est temps de rentrer dans le vif du sujet et de créer notre première application Ionic !

### Installer Ionic
Commençons par installer notre environnement de travail.
- [node.js](https://nodejs.org/en/)
- Les CLI pour cordova et Ionic avec `npm install -g ionic cordova`.

### Créer un projet Ionic
Pour commencer une application avec Ionic, il suffit d'utiliser les outils en ligne de commande. Essayons la commande `ionic templates`. Celle-ci liste une partie des templates officiels disponibles. Sachez qu'il en existe une très grande quantité [trouvables notamment sur Github](https://github.com/search?utf8=%E2%9C%93&q=starter-ionic).

Pour avoir un bon aperçu de ce que propose le framework, nous allons partir d'un template assez complet avec `ionic start myApp tabs`. À ce stade, nous avons un répertoire contenant le patron minimum de notre future application.

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

- des fichiers de configuration pour les outils : bower.json, gulpfile.js, package.json
- des fichiers de configuration pour cordova : config.xml, ionic.project

Nous reviendrons plus tard sur les répertoires `hooks` et `plugins`. Il ne reste donc plus qu'à voir les fichiers sources contenus dans les répertoires `scss` et surtout `www`.

C'est dans ce dernier que nous allons développer l'application web qui deviendra grâce à Ionic et Cordova une application hybride pour Android ou iOS. Pour en avoir un aperçu, après tout cela reste du web, nous pouvons lancer la commande `ionic serve -l` depuis le répertoire `myApp`. Une page dans notre navigateur va s'ouvrir pour présenter les deux versions dans des cadres dédiés à chaque plateforme. (`-l` correspondant à l'option lab, qui vous permet d'observer les 2 versions simultanément.)

Et voilà ! Nous pouvons dors et déjà interagir avec notre nouvelle application. L'un des très grand avantages de cette fonctionnalité est que la page se met à jour automatiquement lorsque nous effectuons des changements dans nos templates, nos scripts, ou nos feuilles de style.

### Ajouter des plugins avec Ionic
Revenons au répertoire `plugins` ; ce dernier stocke, vous l'aurez deviné, les plugins de notre application. Ces éléments vont nous permettre une interaction entre le smartphone et l'application web. Cette dernière tourne dans une webview et les plugins seront le lien entre cette webview et les API des différents outils disponibles dans le SDK du smartphone de vos utilisateurs. Lançons donc `ionic plugin` pour découvrir la liste des plugins déjà installés :

```
$ ionic plugin
cordova-plugin-console 1.0.1 "Console"
cordova-plugin-device 1.0.1 "Device"
cordova-plugin-splashscreen 2.1.0 "Splashscreen"
cordova-plugin-statusbar 1.0.1 "StatusBar"
cordova-plugin-whitelist 1.0.0 "Whitelist"
ionic-plugin-keyboard 1.0.8 "Keyboard"
```

En prenant le dernier élément comme exemple, `ionic-plugin-keyboard`, voici ce que dit le README qui se trouve dans `./plugins/ionic-plugin-keyboard/README.md` :

> The `cordova.plugins.Keyboard` object provides functions to make interacting with the keyboard easier, and fires events to indicate that the keyboard will hide/show.

Concernant le `cordova-plugin-whitelist`, voici ce qu'explique son fichier `README.md` :

> This plugin implements a whitelist policy for navigating the application webview on Cordova 4.0

Voilà donc le principe. Les plugins nous permettent d'accéder ce qui est géré en dehors de la webview. Et si nous avons besoin d'accéder à l'appareil photo du smartphone, nous pouvons utiliser la commande : `ionic plugin add cordova-plugin-camera`.

### Développer l'application
Nous pouvons désormais écrire et améliorer notre application. Ionic utilise AngularJS comme base principale ce qui fait qu'il est quasi indispensable de s'en servir. Mais, pas de panique, ce n'est pas si compliqué de faire des choses simples avec ; si vous découvrez le framework, vous apprendrez plein de choses assez rapidement, et apprendre c'est bien !
De plus, les développeurs du ce merveilleux outil nous ont préparé une liste importantes de [composants](http://ionicframework.com/docs/api/) et d'[éléments](http://ionicframework.com/docs/components/) prêts à l'emploi et documentés ! Il y a déjà de quoi créer la plus part des applications, et c'est plus au niveau de la customisation qu'il vous faudra passer du temps.
Enfin, il est important de noter que la gestion de la navigation et de l'arborescence est gérée par le framework [UI-Router](https://github.com/angular-ui/ui-router), une extension très prisée à AngularJS.

### Déployer une application Ionic
Que serait une application pour smartphone si l'on ne la fait pas tourner dans un smartphone ? Ici nous entrons dans le monde de Cordova, mais comme Ionic est un bel outil, il reprend la plupart des commandes pour ne pas avoir à se poser la question : est-ce que c'est du Ionic ou du Cordova ?

Ici et comme sur les smartphones, il y a deux mondes : Android et iOS. Pour chacun de ces deux mondes, il faut installer les outils de développement spécifiques à chacun avant de pouvoir émuler ou compiler l'application. Pour installer les outils iOS, il faut être dans la sphère Apple et donc posséder une machine avec OSX et XCode. Pour [installer les outils Android](https://developer.android.com/sdk/installing/index.html?pkg=tools), c'est un peu plus ouvert car basé sur java, et nous avons donc la possibilité d'utiliser au choix, Windows, OSX ou une distribution Linux.

Une fois l'environnement prêt, nous pouvons générer les fichiers nécessaires à la plateforme désirée grâce aux commandes `ionic platform add android` ou `ionic platform add ios`.

Vous pouvez alors packager votre application pour la tester soit sur un émulateur, soit sur un périphérique réel. De manière générale il est toujours préférable de tester sur de vrais smartphones et tablettes, notamment à cause de la  lenteur et l'imperfection des émulateurs fournis.

Pour packager : `ionic build android` ou `ionic build ios`
Pour émuler : `ionic emulate android` ou `ionic emulate ios`

Pour déployer sur un périphérique de test, cela diffère selon la plateforme :
Android : `ionic run android` *Cette commande lancera la version debug de notre application sur le premier périphérique trouvé, et si aucun n'est trouvé sur une émulateur*

IOS : Il nous faut ouvrir le projet XCode généré dans le dossier `platforms/ios` avec XCODE lui même, puis sélectionner le périphérique souhaité via l'interface et cliquer sur le bouton "Play".

### Tester une application Ionic
Il est important de noter que l'APK généré par le packaging Android peut être envoyé à n'importe qui pour être testé sur son propre appareil mobile. Cet apk se trouve dans `platforms/android/build/outputs/apk/android-debug.apk`
La version IOS quand à elle ne peut être partagée simplement. Il est nécessaire de préparer une version beta de test via l'iTunes Connect et d'y inscrire des personnes via leurs comptes Apple.


## Astuces Ionic

### AngularJS
Quand il s'agit de coder avec le framework AngularJS, nous vous recommandons la lecture des [bonnes pratiques AngularJS par Johnpapa](https://github.com/johnpapa/angular-styleguide).

### Feuilles de style
Ionic est basé en grande partie sur l'utilisation de SASS, et rend notamment beaucoup de choses paramétrables via des variables. Pour cette raison, et bien d'autres, nous vous recommandons d'[activer dès le départ SASS](http://ionicframework.com/docs/cli/sass.html) sur votre projet via la commande `ionic setup sass`

### Composants
Outre les composants que nous avons vu au cour du tutoriel, il en existe énormément. Bien que Github soit une source intarissable, voici un petit article recensant beaucoup de choses intéressantes !
http://mcgivery.com/100-ionic-framework-resources/

### Plugins
L'utilisation de [ngCordova](http://ngcordova.com/) est fortement conseillée. Il s'agit d'un ensemble de plugins choisis et maintenus par l'équipe d'Ionic, et donc toujours compatibles avec le framework.

### Symboles
Ionic embarque de base [Ionicons](http://ionicons.com/), une banque d'icônes très complète avec des déclinaisons par plateforme. N'oubliez pas de l'utiliser !

### Splashscreens and icons
Ionic offre également [un service](http://ionicframework.com/docs/cli/icon-splashscreen.html) permettant de générer très rapidement tous les formats d'icônes et de splashscreens nécessaire au déploiement sur les deux plateformes. Pour cela ils vous suffit de placer une image pour les icônes et une pour les splashscreens, aux bonnes dimensions, et de lancer la commande  `ionic resources`

### Compatibilité
La compatibilité avec les vieux périphériques Android peut être un vrai problème. À ce problème Ionic nous propose d'utiliser [Crosswalk](https://crosswalk-project.org/), et d'[embarquer au sein de l'application une version récente de Chrome](http://blog.ionic.io/crosswalk-comes-to-ionic/) afin d'assurer un rendu et des performances optimales. Attention cependant, cela n'est utile que si votre but et de toucher les anciennes versions d'Android. Car cet ajout a un coup en terme de poids non négligeable. (plusieurs dizaines de Mo). Pour se faire, il suffit d'entrer la commande `ionic browser add crosswalk`. Attention cependant, l'outil ne permet pas encore la génération de format 9-patch pour les splashscreens Android.

### Tests
Afin de faciliter ce processus, les développeurs nous ont gratifié de l'application [Ionic View](http://view.ionic.io/) qui permet d'envoyer et de manager des versions de tests sur plusieurs périphériques. Cela nous permet de simplement partager l'application, et ce quelque soit la pateforme où l'on désire tester.


## Annexes

- [mindmup](https://www.mindmup.com/#m:a1528691f0719d01333c0303d675c6c013)
- [ionic2 getting started](http://ionicframework.com/docs/v2/getting-started/installation/)
- [building cross platform apps with ionic2 - Adam Bradley @ angularconnect 2015](https://www.youtube.com/watch?v=bAlydPwFONY)