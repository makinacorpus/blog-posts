# Les nouveautés d'Ionic 2 - Retours de l'AngularConnect

Au sein des nombreuses conférences d'AngularConnect, nous avons eu droit à l'annonce de la version 2 du framework hybride mobile le plus utilisé. Avec plus d'un million d'applications créées, Ionic a su écouter ses utilisateurs et nous revient avec de belles nouveautés.


## Nos impressions
Des choses très prometteuses ont été annoncées lors de cette conférence. Parmi elles nous retiendrons particulièrement la *simplification du scripting et du templating*, le passage à la *WAPI pour les animations*, et le *changement du système de routing*. Ces 3 points à eux seuls résolvent des problèmes récurrents et assez importants que l'on recontre dans la v1 d'Ionic. Autant dire que couplé à l'augmentation perpétuelle des performances mobiles, l'hybride à de beaux jours devant lui !


## De manière générale
- Utilisation des capacités des navigateurs les plus récents.
- Simplification du framework en revenant à du Javascript plus standard et du balisage simplifié.
- Utilisation d'[Angular2](https://angular.io/) mais de manière _plus discrète_ que dans la première version.
- Utilisation des nouveautés Javascript.

## Script et template
- Simplification des templates en essayant de se débarasser des classes.
- Favorisation des attributs comme paramètres pour les directives.
- Ajout des décorateurs pour simplifier la création de pages et d'applications.
- Permettre d'écrire les scripts en vanilla JS en limitant au maximum la nécessité de l'utilisation d'Angular.

## Platformes
- Une meilleur séparation des plateformes au coeur du framework pour permettre une syntaxe unique interprétée automatiquement selon le contexte d'exécution.
- Plus que de simples différences de style entre les plateformes, telles que des widgets au comportement plus proches de chaque plateforme.
- Une meilleure réutilisation de l'environnement natif (transitions, onglets, …).
- Plus de 900 icones svg.
- Utilisation des icones avec un code commun à toutes les plateformes.

## Navigation
- Remplacement de la navigation ui-router par un système de Push / Pop.
- Une meilleure gestion des profondeurs de navigation. Notamment avoir une navigation de niveau zéro constante. (exemple : ne plus avoir un bouton retour lorsque l'on passe d'un lien d'un menu latéral à un autre).
- Améliorer la gestion d'urls et des liens profonds.

## Customisaton
- Gestion de plusieurs thèmes avec des propositions par défaut. (exemple : thème sombre).
- SASS activé par défaut.
- Augmentation des éléments modifiables via les variables SASS.
- Utilisations des maps pour gérer les groupes de variables SASS :
<pre>
$light:                           #fff !default;
$stable:                          #f8f8f8 !default;
$positive:                        #387ef5 !default;
$calm:                            #11c1f3 !default;
$balanced:                        #33cd5f !default;
$energized:                       #ffc900 !default;
$assertive:                       #ef473a !default;
$royal:                           #886aea !default;
$dark:                            #444 !default;
</pre>
devient
<pre>
$colors: (

  primary:    #387ef5,
  secondary:  #32db64,
  danger:     #f53d3d,
  light:      #f4f4f4,
  dark:       #222,

);
</pre>

## Animations
- Utilisation de la Web animation API ([normée par le W3C](https://w3c.github.io/web-animations/)) pour gérer les animations.
- WAPI disponible nativement sur Chrome (Android) et avec un polyfill déjà fonctionnel sur IOS (Apple).
- Utilisation de l'accélération matérielle pour les transitions.
- Possibilité d'étendre, d'ajouter et de paramétrer les animations.
- WAPI accessible via une encapsulation simplifiée.

## Configuration
- [Configure all the things!](http://www.google.fr/imgres?imgurl=https%3A%2F%2Fwww.sysorchestra.com%2Fcontent%2Fimages%2F2014%2FJul%2Fconfigure_all_the_things_banner.jpg&imgrefurl=https%3A%2F%2Fwww.sysorchestra.com%2F2014%2F07%2F19%2Frunning-ghost-fast-and-secure-with-debian-wheezy-nginx-and-mariadb%2F&h=200&w=780&tbnid=RMH_sZWcL1YAuM%3A&docid=HBrsLUdgBfyzqM&ei=YhkpVr3gDMXcUa2rsLAI&tbm=isch&iact=rc&uact=3&dur=5961&page=1&start=0&ndsp=39&ved=0CCQQrQMwAWoVChMIvZnL_MnWyAIVRW4UCh2tFQyG)
- Configuration possible à de nombreux niveaux.
- Configuration globale.
- Configuration des plateformes.
- Configuration des attributs.
- Configurations des instances de composants.

## JS
- Passage de ES5 à [ES2015](http://www.ecma-international.org/ecma-262/6.0/index.html) pour la v2.
- [Typescript](http://www.typescriptlang.org/) (optionnel).
- Decorateurs ES2015.

## Accès au téléphone (plugins Cordova)
- intégration de Ng-Cordova au coeur d'Ionic pour les composants les plus importants : batterie, GPS, Bluetooth, appareil photo, …
- Accès simplifié aux fonctionnalités.
- Chargement optimisé pour n'avoir que ce qui est nécéssaire.

## CLI
- ES2015 et Typescript Transpilation.
- Packaging [Webpack](https://webpack.github.io/).

## A venir dans la v2
- Support des [Web workers](http://www.w3.org/TR/workers/).
- Rester à jour avec Angular2.
- Et bien plus…


Vous pouvez d'ores et déjà tester [la version 2.alpha disponible ici](http://ionicframework.com/docs/v2/) !

Et bien sûr la vidéo de la conférence est disponible sur Youtube : [AngularConnect - Building cross-platform apps with Ionic 2](https://youtu.be/43vanF4YwRI?t=47m20s)
