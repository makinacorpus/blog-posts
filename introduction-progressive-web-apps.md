---
title: Bien comprendre les Progressive Web Apps
description: On va beaucoup en parler, voilà de quoi suivre les
discussions.
url: /blog/metier/2016/introduction-progressive-we
creators: eco
---

Chez Makina Corpus, [nous faisons du mobile](http://makina-corpus.com/realisations/application-mobile-meteo-indonesie),
mais ce que nous aimons par dessus tout, c'est le web.
C'est pourquoi [nous utilisons Cordova et Ionic](http://makina-corpus.com/blog/metier/2015/bien-demarrer-avec-ionic)
pour porter les applications de nos clients sur les stores
Android et iOS.

_N.B._ [React Native](http://makina-corpus.com/blog/metier/2016/decouverte-de-react-native)
ou des solutions comme NativeScript nous
plaisent un peu moins, car bien qu'elles utilisent des langages
connus des développeurs Web, nous sortons du Web et nous n'avons
plus accès à notre [Web API](https://developer.mozilla.org/en-US/docs/Web/API)
que nous aimons tant.

# Native versus Web App

Pour autant, malgré tous nos efforts, les Web Apps
ne fournissent pas tout à fait la même expérience que les
applications hybrides :

_« C'était quoi déjà l'url de la Web App pour trouver une radio ? »_

_« Ah mince, j'ai pas de réseaux, je peux pas voir le numéro de téléphone. »_

_« Oh, elle est jolie cette App, mais qu'est-ce qu'elle rame ! »_

Cordova permet déjà de créer une _presque_ application native.
Elle peut être déployée sur les stores.
Elle peut utiliser les fonctionnalités du téléphone :
appareil photo, boussole, gps, …

# Oh wait…

Ces fonctionnalités existent déjà dans le Web.
Vous avez déjà vu ce genre de notifications dans votre navigateur ?

![](https://makina-corpus.com/blog/metier/images/geolocation-API.png)

![](https://makina-corpus.com/blog/metier/images/notifications-API.png)

![](https://makina-corpus.com/blog/metier/images/camera-API.png)

Et le Web ne cesse d'évoluer pour permettre à tout le monde
et tous les périphériques d'en profiter,
pour le pire ou pour le meilleur.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">the future, finally delivered <a href="https://t.co/0b7Vz3BUGm">pic.twitter.com/0b7Vz3BUGm</a></p>&mdash; Internet of Shit (@internetofshit) <a href="https://twitter.com/internetofshit/status/723792156197040129">April 23, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Et c'est à ce moment là qu'on peut commencer à parler des Progressive Web Apps.

# Progressive Web Apps

Ceci est une révolution ? Pas vraiment. Mais nous pouvons quand même accepter
un véritable changement de paradigme en associant plusieurs technologies Web
afin de passer l'expérience utilisateur au niveau supérieur.

Une Progressive Web App est une Web App qui va associer le meilleur du web
avec le meilleur des applications natives.

Prenons un exemple.

Un utilisateur découvre pour la première fois votre application dans un de ses
onglets de [son navigateur préféré](http://mzl.la/1Lu1XwU), hyper accessible
puisqu'aucune installation n'est requise.
Plus l'utilisateur va utiliser l'application qui lui rend pas mal de services,
plus elle va être agréable à utiliser : chargement rapide, même sur un réseau
moyen, envoi de notification utile, disponible directement depuis la page
d'accueil du téléphone et expérience améliorée puisqu'elle prend désormais
toute la place disponible dans l'écran sans les bordures du navigateur.

Ainsi on pourra dire qu'une Progressive Web App c'est :

- Chargement instantané : grâce au [Service Worker](http://makina-corpus.com/blog/metier/2016/decouvrir-le-service-worker), l'accès au réseau
devient optionnel ;
- Rapidité : comme une application native, l'application doit afficher
[60 FPS](https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API) (_Frame Per Second_) grâce aux possibilités offertes pour
gérer les animations, la navigation et les défilements ;
- Notification Push : l'[API Web Push Notification](https://developer.mozilla.org/en-US/docs/Web/API/notification) et le [Service Worker](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
permet de forger l'engagement de l'utilisateur en leur proposant
des notifications contextuelles et adéquates, et ce, même si
le navigateur est fermé ;
- [Responsive](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries) : l'utilisateur accède à votre application par différents
périphériques, [votre application doit faire de même](http://alistapart.com/article/responsive-web-design) et suivre l'utilisateur
sur son téléphone, sa tablette ou son ordinateur (voire sa montre) ;
- Sécurisée : les données de l'utilisateur sont précieuses et il faut
s'en occuper comme il se doit. L'[utilisation de HTTPS](https://letsencrypt.org/) est indispensable
pour le Service Worker, et aussi pour les Progressive Web Apps ;
- Disponible sur l'écran d'accueil : votre application doit être
proche de l'utilisateur, il doit pouvoir y accéder depuis l'écran
d'accueil de son téléphone, comme une application native. C'est à ça
que le [Web App Manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest) va servir.

# La révolution

Vous avez pu vous rendre compte que chaque élément de la liste ci-dessus
point vers une documentation. Si certaines technologies sont en cours
de finalisation, elles sont toutes au moins validées par le W3C.
Ce sont donc bien des technologies, non pas de demain, mais de
tout à l'heure.

On parle encore peu de Progressive Web Apps. Pourtant, chez Makina
Corpus, nous sommes convaincus des qualités du Web et nous commençons
déjà à prendre en compte cette évolution.

Comme à chaque fois, la meilleure façon de comprendre est d'essayer
avec une applicaiton prototype.
Google, le responsable d'Android, fournit un guide très complet
duquel vous pouvez suivre les directives pour [créer votre première
Progressive Web App](https://developers.google.com/web/fundamentals/getting-started/your-first-progressive-web-app/). Et lorsque vous
aurez besoin d'un backend pour servir vos données, jetez donc un
œil à [Kinto](http://www.kinto-storage.org/), un serveur de données
à API REST, qui, associé à [Kinto-client.js](https://github.com/Kinto/kinto.js)
, sera [le parfait allié](http://www.servicedenuages.fr/en/what-can-kinto-do-for-you) de votre Progressive Web App.
