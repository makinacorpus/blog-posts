# Tout savoir avant de créer son application mobile

Peut-être lisez-vous ce billet sur un téléphone mobile.
Si c'est le cas, vous comprenez sûrement l'intérêt
d'être présent ou au moins lisible
sur ces petits terminaux nomades.

![](/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/screenshot-makina-mobile.png)

Si vous pensez que l'accès à internet depuis un
téléphone mobile est un gadget de plus dans la
disruption digitale <sup title="C'est toujours intéressant les buzz words dans un article de blog.">[1](#note1)</sup>, alors vous devriez relire
cet article de 2010 :
[Eric Schmidt: Mobile Is The Future, And There's No Such Thing As Communication Overload – TechCrunch, 12/04/2010](https://techcrunch.com/2010/04/12/eric-schmidt-mobile-is-the-future-and-theres-no-such-thing-as-communication-overload/).

L'ancien patron de Google a eu le nez creux car si l'usage du
téléphone mobile est très courant dans le couloir des métros des
grandes métropoles, les
[Nations Unies ont récemment fait un rapport](http://www.un.org/africarenewal/magazine/april-2014/internet-access-no-longer-luxury)
qui va dans le même sens :
la croissance du mobile en Afrique est passée de
1% en 2000 à 54% en 2012.

La présence sur le monde de l'Internet mobile n'est
plus accessoire.
Ainsi, en 2016, votre réelle problématique devrait être
*"site Web ou application ?"*.
Nous allons voir ensemble comment faire ce choix.

[Luke Wroblewski](https://twitter.com/lukew)
propose une infographie qui se résume assez simplement :

- le Web sert à atteindre l'audience ;
- l'application sert à fournir une expérience riche ;
- les deux sont stratégiques.

![](/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/REACHvsRICH.png)

Selon la plateforme, on n'atteint pas le même but.
Étant donné le prix des développements, mieux vaut ne pas
se tromper.

Si on touche plus de monde sur Android que sur iOS,
on en touche encore plus avec une application Web.

S'il est plus facile de se rémunérer avec une application native
qu'avec une application Web, les utilisateurs d'iPhone rapporte
plus que les utilisateurs d'Android.

[iA, un éditeur de logiciels pour téléphone et tablette en a fait le constat](https://ia.net/writer/updates/the-best-things-in-android-are-freewith-in-app-purchases).
Ils ont changé de stratégie : l'application pour Android
est devenue gratuite, mais il faudra acheter des fonctionnalités si
on veut l'utiliser à fond.

![](/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/app-downloads-and-sales-android-vs-ios.png)

L'idée étant vraiment d'atteindre le plus de monde possible
tout en étant rentable.

## Site Web Reponsive

Pour atteindre les utilisateurs nomades, le minimum vital est de
prendre en compte certains principes de *Responsive Design*
<sup title="Celui là aussi est un buzz word que j'avais envie d'utiliser, même s'il tombe en désuétude.">[2](#note2)</sup>
pour rendre votre site Web accessible depuis un mobile.
Mais s'il vous faut plus d'interactivité avec l'utilisateur, il faudra
trouver une autre solution.
N.B. :
L'[article de MDN sur le Responsive Design](https://developer.mozilla.org/en-US/docs/Web_Development/Responsive_Web_design)
a été archivé fin 2015.

## Applications Natives

Une application native [coûte très chère](http://howmuchtomakeanapp.com/).
Et s'il faut la déployer sur les deux stores principaux
([Play Store](https://play.google.com/store)
et [App Store](https://itunes.apple.com/us/genre/ios/id36?mt=8)), il faudra
la développer deux fois.
Bien qu'étant le choix par défaut depuis 2007, outre l'investissement,
on peut y trouver de nombreuses limitations :

- Deux langages différents pour [Android](https://developer.android.com/index.html) et [iOS](https://developer.apple.com/ios/) ;
- Autoritarisme des stores lors des publications ;
- Maintenance lourde.

## Applications Natives précompilées

Pour pallier le besoin de développer deux, voire trois fois, une
application, certains éditeurs proposent un langage et un compilateur
permettant de concrétiser la maxime "Write Once, Run Everywhere"
(*Écrire une fois, Faire tourner partout* <sup title="Ça sonne quand même mieux en anglais, sauf l'acronyme.">[3](#note3)</sup>).
On y retrouve [React Native](https://facebook.github.io/react-native/),
[NativeScript](http://www.telerik.com/nativescript).

Mais si on peut effectivement écrire un seul code, il faudra quand même
faire avec les spécificités des SDKs pour construire les interfaces graphiques.

## Applications Hybrides

Cette fois, l'idée principale de ces solutions est de proposer aux
équipes de développeurs Web de profiter de leur expertise pour créer
des applications hybrides.
Ce terme définit l'utilisation des technologies du Web (HTML, JavaScript, CSS)
embarquée dans [une WebView](https://cordova.apache.org/docs/en/latest/guide/hybrid/webviews/index.html)
(souvent une [WebView CrossWalk](https://crosswalk-project.org/)).

Des plugins développés dans le langage du SDK cible permettent aux applications
hybrides de communiquer avec le reste de l'appareil
(accès aux fichiers, aux différents périphériques, appareil photo, puce GPS, …).

On notera dans cet écosystème la présence de
[Drifty qui produit Ionic Framework](http://ionicframework.com/docs/v2/).
Un framework basé sur Angular 2 et Cordova pour construire des applications
Web hybrides de grande qualité.

Mais il faudra toujours passer par les stores.

## Progressive Web Apps

C'est ce qui vient en lieu et place d'une Web app Responsive.

On appellera [Progressive Web App](http://makina-corpus.com/blog/metier/2016/introduction-progressive-web-apps)
une application qui utilise correctement
les dernières spécifications HTML5 et mobile.
Les développeurs Web, sous couvert d'une légère remise à niveau,
se retrouveront en terrain connu.

Ils utiliseront [les Services Workers](http://makina-corpus.com/blog/metier/2016/decouvrir-le-service-worker)
pour couvrir l'utilisation offline,
permettre l'émission de notifications,
mettre à jour l'application en arrière-plan.

Grâce aux [Web Animations](https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API),
les applications pourront proposer des animations en 60 FPS <sup title="60 FPS veut dire qu'il y a 60 images peintes en une seconde, c'est vraiment *smooth*.">[4](#note4)</sup>.

On pourra les installer grâce à un [Manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest).
Le gros avantage dans ce cas est de proposer une expérience progressive
aux utilisateurs qui visiteront la Web App plusieurs fois :
très accessible, l'utilisateur novice viendra visiter la web app ;
mais s'il devient coutumier, il pourra *installer* l'application
très facilement sur son téléphone.

Pour en savoir plus, n'hésitez pas à visiter
[notre article](http://makina-corpus.com/blog/metier/2016/introduction-progressive-web-apps),
le dossier de [Google](https://developers.google.com/web/progressive-web-apps/)
ou celui du
[Mozilla Developer Network](https://developer.mozilla.org/en-US/Apps/Progressive).

Et si HTML5 nous permet de communiquer avec les périphériques de l'appareil mobile
(GPS, Appareil Photo, …), l'utilisateur pourra être rassuré de ne pas se voir
demander une liste impréssionnante d'autorisations pour une application de
calculatrice.

![](/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/toomanypermissions.png)

L'autre avantage de la Progressive Web App est son coût de développement
qui sera plus proche d'une Web App Responsive que de deux applications natives.

## Intégrer le monde mobile

Il n'y a pas de bonne réponse à la question *"que dois-je faire pour intégrer
le monde mobile ?"*.
La seule bonne réponse à ce genre de question est :

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">any decent answer to an interesting question begins, &quot;it depends...&quot;</p>&mdash; Kent Beck (@KentBeck) <a href="https://twitter.com/KentBeck/status/596007846887628801">May 6, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Selon votre besoin, nous pourrons vous aider à définir la meilleure stratégie,
toujours selon [le principe KISS](https://fr.wikipedia.org/wiki/Principe_KISS).

Pourquoi ne pas commencer par vous posez ces quelques questions
(cliquez sur l'image pour la voir en grand) :

[![](/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/AppMobileBlueprint.jpg)](/blog/metier/2016/images/tout-savoir-avant-creer-application-mobile/AppMobileBlueprint.jpg)

---

<a name="note1">1</a> : C'est toujours intéressant les buzz words dans un article de blog.

<a name="note2">2</a> : Celui là aussi est un buzz word que j'avais envie d'utiliser, même s'il tombe en désuétude.

<a name="note3">3</a> : Ça sonne quand même mieux en anglais, sauf l'acronyme.

<a name="note4">4</a> : 60 FPS veut dire qu'il y a 60 images peintes en une seconde, c'est vraiment *smooth*.

[Illustration: [matthew venn CC by-sa](https://flic.kr/p/5BkUwX)]

Une erreur, une question, une remarque,
n'hesitez pas : [@ticabri](https://twitter.com/ticabri)
ou directement par une [pull-request](https://github.com/makinacorpus/blog-posts/blob/master/tout-savoir-creer-application-mobile.md).