# Tout savoir avant de créer son application mobile

Peut-être lisez-vous ce billet sur un téléphone mobile.
Si c'est le cas, vous comprenez sûrement l'intérêt d'être présent ou au moins lisible sur ces petits terminaux nomades.

Si vous pensez que l'accès à Internet depuis un téléphone mobile est un gadget de plus dans la disruption digitale <sup title="C'est toujours intéressant les buzz words dans un article de blog.">[1](#note1)</sup>, alors vous devriez relire cet article de 2010 :
[Eric Schmidt: Mobile Is The Future, And There's No Such Thing As Communication Overload – TechCrunch, 12/04/2010](https://techcrunch.com/2010/04/12/eric-schmidt-mobile-is-the-future-and-theres-no-such-thing-as-communication-overload/).

L'ancien patron de Google a eu le nez creux car si l'usage du téléphone mobile est très courant dans le couloir des métros,
les [Nations Unies ont récemment fait un rapport](http://www.un.org/africarenewal/magazine/april-2014/internet-access-no-longer-luxury) qui va dans le même sens :
la croissance du mobile en Afrique est passée de 1% en 2000 à 54% en 2012.

Être présent sur l'Internet mobile n'est plus accessoire.
En 2016, la problématique réelle devrait être *"site Web ou application ?"*.
Nous allons voir ensemble comment faire ce choix.

<img style="width:65%;display:block;margin:auto" src="/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/screenshot-makina-mobile.png" >

[Luke Wroblewski](https://twitter.com/lukew) propose une infographie qui se résume assez simplement :

- le Web sert à atteindre l'audience ;
- l'application sert à fournir une expérience riche ;
- les deux sont stratégiques.

<img style="width:65%;display:block;margin:auto" src="/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/REACHvsRICH.png" >

Dans le schéma ci-dessus, Wroblewski oppose l'utilisation riche des applications (à gauche) à la facilité d'accès du Web (à droite).

Selon la plateforme, on n'atteint pas le même but.
Étant donné le prix des développements, mieux vaut ne pas se tromper.

Si on touche plus de monde sur Android que sur iOS,
on en touche encore plus avec une application Web.

S'il est plus facile de se rémunérer avec une application native qu'avec une application Web,
les utilisateurs d'iPhone rapportent plus que les utilisateurs d'Android.

iA, un éditeur de logiciels pour téléphones et tablettes en a fait [le constat](https://ia.net/writer/updates/the-best-things-in-android-are-freewith-in-app-purchases).
Ils ont changé de stratégie :
l'application pour Android est devenue gratuite,
mais il faudra acheter des fonctionnalités si on veut l'utiliser à fond.

<img style="width:65%;display:block;margin:auto" src="/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/app-downloads-and-sales-android-vs-ios.png" >

L'idée étant vraiment d'atteindre le plus de monde possible tout en étant rentable.

Alors quels sont les choix possibles lorsqu'on veut être présent sur mobile ?

## Site Web Responsive

Le minimum vital est d'appliquer certains principes de *Responsive Design* <sup title="Celui là aussi est un buzz word que j'avais envie d'utiliser, même s'il tombe en désuétude.">[2](#note2)</sup> pour rendre votre site Web accessible depuis un mobile.

_N.B. : L'[article de MDN sur le Responsive Design](https://developer.mozilla.org/en-US/docs/Web_Development/Responsive_Web_design) a été archivé fin 2015, preuve de la désuétude du terme._

Il est indispensable de prendre en compte les différentes tailles d'écran.
Il faudra adapter les éléments du Site Web en utilisant des pourcentages pour définir leurs tailles.
Il faudra également éviter de charger une image en haute résolution si cette dernière doit s'afficher sur un petit écran basse résolution
– permettant aussi de soulager la bande passante de l'utilisateur.

Mais s'il vous faut plus d'interactivité avec l'utilisateur, il faudra peut-être trouver une autre solution.
Par exemple, il est facile et rapide d'écrire avec un clavier physique sur un ordinateur fixe,
mais il est difficile de prendre une photo de son environnement ; et inversement avec un téléphone mobile.

## Applications Natives

Une autre possibilité pour être présent sur mobile est le développement d'une application native.
Mais une application native [coûte très cher](http://howmuchtomakeanapp.com/).
Et s'il faut la déployer sur les deux stores principaux ([Play Store](https://play.google.com/store) et [App Store](https://itunes.apple.com/us/genre/ios/id36?mt=8)),
il faudra la développer deux fois.
Bien qu'étant le choix par défaut depuis 2007, outre l'investissement,
on peut y trouver de nombreuses limitations :

- Deux langages différents pour [Android](https://developer.android.com/index.html) et [iOS](https://developer.apple.com/ios/) ;
- Autoritarisme des stores lors des publications ;
- Maintenance lourde.

## Applications Natives précompilées

Pour pallier le besoin de développer deux applications – voire trois, en incluant un développement Windows Phone –
certains éditeurs proposent des solutions permettant de concrétiser la maxime *« Write Once, Run Everywhere »*
(*Écrire une fois, Faire tourner partout* <sup title="Ça sonne quand même mieux en anglais, sauf l'acronyme.">[3](#note3)</sup>).
C'est le cas de [React Native](https://facebook.github.io/react-native/) ou de [NativeScript](http://www.telerik.com/nativescript).

Mais si on peut effectivement écrire un seul code,
il faudra quand même faire avec les spécificités des SDKs pour construire les interfaces graphiques.
Et ce n'est pas toujours simple.

## Applications Hybrides

Pour des développeurs Web, il est plus facile de développer des applications avec les technologies du Web : HTML, CSS et JavaScript.
L'idée principale des solutions hybrides est de profiter des expertises des développeurs Web pour [créer des applications mobiles](http://makina-corpus.com/blog/metier/2016/quelle-solution-pour-mon-application-mobile-hybride).
C'est ce que propose Cordova – et les distributions telles que PhoneGap ou Ionic –
qui permet d'embarquer [une WebView pleine page](https://cordova.apache.org/docs/en/latest/guide/hybrid/webviews/index.html)
(souvent une [WebView CrossWalk](https://crosswalk-project.org/)) dans une application native.

Des plugins développés dans le langage du SDK cible (Java pour Android et Objective-C/Swift pour iOS) permettent aux applications hybrides de communiquer avec le reste de l'appareil
(accès aux fichiers, aux différents périphériques, appareil photo, puce GPS, etc.).

Mais il faudra toujours passer par les stores et leurs validations parfois obscures et souvent péremptoires.

## Progressive Web Apps

On appelle [Progressive Web App](http://makina-corpus.com/blog/metier/2016/introduction-progressive-web-apps) une application qui utilise correctement les dernières spécifications HTML5 et mobile.
Les développeurs Web, sous couvert d'une légère remise à niveau, se retrouveront en terrain connu.

Ils utiliseront [les Services Workers](http://makina-corpus.com/blog/metier/2016/decouvrir-le-service-worker) pour couvrir l'utilisation offline, permettre l'émission de notifications, mettre à jour l'application en arrière-plan.

L'utilisateur n'installera pas l'application sur son téléphone, mais l'application mettra en cache ce qui lui est nécessaire pour fonctionner.
Cette mise en cache permet aussi d'éviter de télécharger *« l'App Shell »*, l'ensemble des éléments graphiques qui formeront l'interface de l'application.

Grâce aux [Web Animations](https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API), les applications pourront proposer des animations en 60 FPS <sup title="60 FPS veut dire qu'il y a 60 images peintes en une seconde, c'est vraiment *smooth*.">[4](#note4)</sup>.
Ces animations permettent à l'utilisateur d'avoir la même expérience graphique qu'une application native en déportant tous ces calculs à un processeur graphique désormais présent sur la grande majorité des terminaux mobiles.

La présence d'un [Manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest) permet d'accompagner l'utilisateur au-delà de sa première visite.
Il propose une expérience progressive aux utilisateurs qui visiteront la Web App plusieurs fois :
le manifest définit un icône et un *splash screen* et permet à l'utilisateur d'ajouter cette Web App à son lanceur d'applications.

Pour en savoir plus, n'hésitez pas à visiter
[notre article](http://makina-corpus.com/blog/metier/2016/introduction-progressive-web-apps),
le dossier de [Google](https://developers.google.com/web/progressive-web-apps/)
ou celui du
[Mozilla Developer Network](https://developer.mozilla.org/en-US/Apps/Progressive).

HTML5 nous permet d'utiliser [la plupart des fonctionnalités](https://whatwebcando.today/) d'un appareil mobile (GPS, Appareil Photo, bluetooth, etc.).

[<img style="width:65%;display:block;margin:auto" src="/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/lost-users.png" >](https://youtu.be/qmE_jpnYXFo?t=96)

Comme précisé plus haut, la publication d'une application native sur un store peut être une étape difficile.
Avec une PWA, c'est une étape inutile, les utilisateurs qui visitent votre site n'auront pas besoin d'accepter des permissions, de télécharger votre application.
Et on trouve déjà des sites qui remplacent les *stores* pour référencer les applications PWA comme [https://pwa.rocks/](https://pwa.rocks/).

Voici par exemple les permissions demandées pour une simple application de lampe de poche :

<img style="width:65%;display:block;margin:auto" src="/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/toomanypermissions.png" >

L'autre avantage de la Progressive Web App est son coût de développement qui sera plus proche d'une Web App Responsive que de deux applications natives.

[Dan Dascalescu](https://twitter.com/dandv) résume très bien la situation [des PWA face aux applications natives](https://medium.com/@dandv/why-progressive-web-apps-vs-native-is-the-wrong-question-to-ask-fb8555addcbb#.e5rg71kjm) :

> It’s not “PWA vs. native”, but rather “PWA vs. [web + native + native]”.

## Intégrer le monde mobile

Il n'y a pas de bonne réponse à la question *"que dois-je faire pour intégrer le monde mobile ?"*.
La seule bonne réponse à ce genre de question est :

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">any decent answer to an interesting question begins, &quot;it depends...&quot;</p>&mdash; Kent Beck (@KentBeck) <a href="https://twitter.com/KentBeck/status/596007846887628801">May 6, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Selon votre besoin, nous pourrons vous aider à définir la meilleure stratégie, toujours selon [le principe KISS](https://fr.wikipedia.org/wiki/Principe_KISS).

Pourquoi ne pas commencer par vous poser ces quelques questions (cliquez sur l'image pour la voir en grand) :

[<img style="width:65%;display:block;margin:auto" src="/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/AppMobileBlueprint.jpg" >](/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/AppMobileBlueprint.jpg)

---

<a name="note1">1</a> : C'est toujours intéressant les buzz words dans un article de blog.

<a name="note2">2</a> : Celui là aussi est un buzz word que j'avais envie d'utiliser, même s'il tombe en désuétude.

<a name="note3">3</a> : Ça sonne quand même mieux en anglais, sauf l'acronyme.

<a name="note4">4</a> : 60 FPS veut dire qu'il y a 60 images peintes en une seconde, c'est vraiment *smooth*.

[Illustration: [matthew venn CC by-sa](https://flic.kr/p/5BkUwX)]

Une erreur, une question, une remarque,
n'hesitez pas : [@ticabri](https://twitter.com/ticabri)
ou directement par une [pull-request](https://github.com/makinacorpus/blog-posts/blob/master/tout-savoir-avant-creer-application-mobile.md).
