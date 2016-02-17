# Quelle solution pour mon application hybride ?

Le Javascript avance, et vite. C'est un peu le leitmotiv du développement front-end depuis quelques années. Une conséquence est cela est là : l'essort des applications mobiles hybrides et des solutions diverses et variées y répondant.

Je vous propose un petit tour d'horizon des grandes solutions gratuites disponibles en ce début 2016, avec leurs avantages et inconvénients respectifs.

## Cordova : le senior
Cordova est là depuis longtemps. Il a pavé la route de bonnes intentions, vers une uniformisation de l'accès aux fonctions natives des plateformes mobiles. Non sans surprise il reste aujourd'hui l'un des grands pilliers du mobile hybride. Non seulement il continue d'être maintenu et amélioré très régulièrement (sortie de la version 6 le 28 Janvier 2016 !), mais surtout il sert de base à de nombreux frameworks dont Ionic dont nous parlerons plus tard.

Il est facile de se dire que Cordova seul n'est pas "assez", mais il convient parfaitement à certains types d'application où la lourdeur d'un framework complet est une chose dont on peut largement se passer.

### Points positifs :
* Compatible avec de très nombreuses plateformes (Android, IOS, Windows Phone, Blackberry, Ubuntu mobile, Firefox OS, LG web OS, FireOS) ;
* Liberté technologique. (Utilisez le framework qui vous convient, ou pas du tout de framework d'ailleurs !) ;
* Performances entièrement dépendantes de ce que l'on en fait ;
* Une base de plugins très complète et bien maintenue ;
* Technologie mature ;
* Open-source.

### Points négatifs :
* Demande un travail plus conséquent pour avoir un résultat de production ;
* Interface graphique non comprise ;
* Abscence de visualisation hors émulateur/périphérique par défaut.


## Ionic : le petit frère
Ionic est aujourd'hui l'un des frameworks les plus en vogue pour faire des applications hybrides rapidement et de qualité. Depuis sa création il y a environ 2 ans, l'outil n'a cessé de s'améliorer et de fédérer autour de lui. Il partage actuellement le haut du panier avec React-Native en terme d'engouement, et la version 2 à venir (déjà disponible en beta) ne fait qu'amplifier ce phénomène. L'attrait principal du framework est qu'il offre toute une interface prête à utiliser, au sein d'un système de routage et d'outils complets. Basé sur Cordova, il profite des plugins et offre donc une surcouche à celui-ci. Cette surcouche utilisant majoritairement AngularJS.

### Points positifs :
* UI complète (avec un système de templating unifié pour toutes les plateformes) ;
* Création rapide pour la production ;
* Basé sur AngularJS ;
* CLI très puissant (ex: visualisation dans le navigateur avec du live reload) ;
* dépôt de plugins cordova spécifiquement maintenu par l'équipe du framework.

### Points négatifs :
* Compatible uniquement iOS et Android (Windows Phone prévu pour la version 2) ;
* Basé sur AngularJS ;
* Système de routing vite limité pour des navigations complexes (point adressé dans la version 2) ;
* Performances encore assez éloignées du natif.


## React-Native : le cousin des states
React-Native est le penchant mobile du framework de Facebook, React. Encore en cours de finalisation, il apporte une autre réponse au fameux mythe du "écrire une fois et déployer partout".
L'intérêt principal est qu'il utilise la même syntaxe que React, et permet donc, modulo les éléments spécifiques aux plateformes mobiles, de réutiliser le code pour une version desktop, tout en créant des versions natives pour Android et iOS. Il devient d'ailleurs même envisageable, avec un peu de réécriture, de l'associer à un cordova pour le déployer sur d'autres plateformes que celles supportées par le framework.

### Points positifs :
* Performances natives ;
* Framework édité par Facebook ;
* Logique de composants ;
* Même syntaxe que React (donc possibilité de sortir une version web).

### Points négatifs :
* Framework jeune ;
* Compatibilité Android partielle ;
* N'est pas du web à proprement parlé et donc demande une adaptation pour les développeurs ;
* Framework édité par Facebook.
