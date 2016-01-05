# Découvrir le Service Worker

## Si vous n'en avez jamais entendu parlé

Un Service Worker est un script chargé parallèlement aux scripts de votre page et qui va s'exécuter en dehors du contexte de votre page web. Bien que le Service Worker n'a pas accès au DOM ou aux interactions avec l'utilisateur, il va pouvoir communiquer avec vos scripts via l'[API postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Client/postMessage). Il se place en proxy de votre Web App, interceptant toutes les requêtes serveur et propose par exemple d'y répondre avec un cache ou en récupérant des données du [LocalStorage](https://developer.mozilla.org/en-US/docs/Web/API/Storage/LocalStorage) ou d'[IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API). Il rend donc votre application [compatible offline](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers#The_premise_of_Service_Workers).

## Les promesses des Service Workers

Que nous soyons dans un contexte d'application mobile hybride ou de webapp responsive, la mobilité des terminaux d'accès traine souvent avec elle son côté obscur : la perte de connectivité (ou la faible connectivité). Sans accès au serveur, nos applications deviennent inutiles et ce n'est pas [l'AppCache qui va venir à notre secours](http://alistapart.com/article/application-cache-is-a-douchebag). [Les Services Workers nous permettent de palier ces problèmes](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers#The_premise_of_Service_Workers) en utilisant des ressources en cache si le réseau n'est pas disponible, permettant ainsi à notre application de fournir une expérience peu dégradée avant de retrouver une réseau disponible.

### A script apart

Le script d'un Service Worker tourne dans le navigateur, mais en arrière-plan, sans accès au DOM ni aux interactions avec les utilisateurs. Il se place entre votre webapp et le réseau, permettant de jouer le rôle de proxy pour nos ressources. Il a une durée de vie indépendante du site web, il s'arrête lorsqu'il n'est pas utilisé et redémarre si besoin. Il n'a pas besoin de votre application pour tourner et peut donc permettre l'envoi de [notifications](https://serviceworke.rs/push-rich.html) (Comme le précise [caniuse.com](http://caniuse.com/#search=service%20worker) il faudra essayer la démo avec chromium ou firefox 44).

### Le cycle de vie d'un Service Worker

Pour installer un Service Worker pour votre site, nous devrons l'enregistrer (__register__) dans le script de notre webapp. Une fois installé, notre Service Worker va s'activer (__activate__), c'est à ce moment que nous allons gérer la mise à jour du cache ou du Service Worker lui-même.

Après l'activation, le Service Worker est prêt à intercepter les événements __fetch__ et __message__ émis respectivement par une requête serveur ou un appel via l'API _postMessage_.

### Découvrir et tester les Services Workers

Mozilla et Google sont à la pointe concernant les Service Workers. Ils proposent tous les deux des recettes et autres _patterns_ pour les utiliser au mieux.

N'hésitez pas à visiter le site [https://serviceworke.rs](https://serviceworke.rs) de Mozilla pour découvrir les Serices Workers en action.

Profitez de l'[Offline Cookbook](https://jakearchibald.com/2014/offline-cookbook/) de [Jake Archibald](https://twitter.com/jaffathecake) pour faire les points sur les cas d'utilisation. Il a même développé [une application pour les besoins de démonstration](https://jakearchibald.github.io/trained-to-thrill/).

## Implémenter son premier Service Worker

### Prérequis

- Utiliser un navigateur récent : Firefox 44+ ou Chromium 47+.
- Repérer l'activation du mode offline dans [Firefox](https://support.mozilla.org/en-US/questions/895486) ou dans [Chromium](http://stackoverflow.com/questions/16091243/does-chrome-have-a-work-offline-option).
- Visiter l'application [https://mdn.github.io/sw-test/](https://mdn.github.io/sw-test/).
- Retrouver le code de l'application [https://github.com/mdn/sw-test](https://github.com/mdn/sw-test)

### Le fichier html ([source](https://github.com/mdn/sw-test/blob/gh-pages/index.html))

L'application charge trois ressources : un fichier de style css, un fichier contenant les données et un fichiers javascript pour générer la page.

### Le fichier de données ([source](https://github.com/mdn/sw-test/blob/gh-pages/image-list.js))

Le fichier déclare une variable dans [le scope global](http://makina-corpus.com/blog/metier/2015/bien-demarrer-avec-javascript#le-scope-global) contenant une liste d'images avec pour chacune : un nom, un texte alternatif, une url et une attribution.

### Le fichier app.js ([source](https://github.com/mdn/sw-test/blob/gh-pages/app.js))

Le premier bloc enregistre le Service Worker avec le code suivant :

```javascript
if ('serviceWorker' in navigator) {
  navigator.serviceWorker
    .register('/sw-test/sw.js', { scope: '/sw-test/' })
    .then(function(reg) {
      // suivre l'état de l'enregistrement du Service Worker : `installing`, `waiting`, `active`
    });
}
```

Le script du Service Worker est situé à l'adresse `/sw-test/sw.js`, nous rentrerons dans les détails dans le prochain paragraphe.

La suite du script récupère le fichier de données et crée pour chacune des entrées de la galerie une balise `<img>` avec l'url de l'image, une balise `<caption>` avec le titre et l'attribution, enfin une figure parente `<figure>` pour encapsuler le tout. Tout cela se passe après que le document a fini de chargé grâce à [`window.onload`](https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/onload).

Ici, rien de particulier, une [requête ajax](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest) récupère les données et l'API [`document.createElement`](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement) permet de créer les différents éléments HTML et de les insérer dans la balise `<section>` du fichier `index.html`.

### Le Service Worker ([source](https://github.com/mdn/sw-test/blob/gh-pages/sw.js))

Au premier appel du Service Worker, ce dernier est installé dans le navigateur de l'utilisateur grâce à :

```javascript
this.addEventListener('install', function(event) {
  // ajouter les fichiers au cache
});
```

`event.waitUntil` permet de bloquer les autres événements jusqu'à la résolution (ou le rejet) des promesses passées en paramètres.

Le Service Worker propose [une interface __cache__](https://developer.mozilla.org/en-US/docs/Web/API/Cache) pour représenter les paires d'objets __Request/Response__ qui seront mises en cache. On peut enregistrer plusieurs objets cache pour un même domaine. Ainsi le code suivant ouvre ‒ s'il existe ‒ ou crée ‒ sinon ‒ le cache _v1_ et y enregistrera, lorsqu'elles seront appelées, les paires de requêtes/réponses correspondant aux routes écrites :

```javascript
caches.open('v1').then(function(cache) {
  return cache.addAll([
    '/sw-test/',
    '/sw-test/index.html',
    '/sw-test/style.css',
    '/sw-test/app.js',
    '/sw-test/image-list.js',
    '/sw-test/star-wars-logo.jpg',
    '/sw-test/gallery/',
    '/sw-test/gallery/bountyHunters.jpg',
    '/sw-test/gallery/myLittleVader.jpg',
    '/sw-test/gallery/snowTroopers.jpg'
  ]);
});
```

_On remarquera l'utilisation des promesses dont [on a déjà évoqué l'utilité](http://makina-corpus.com/blog/metier/2015/bien-demarrer-avec-javascript#le-prochain-javascript)._

Pour intercepter les requêtes, on ajoutera un comportement à l'événement __fetch__ :

```javascript
this.addEventListener('fetch', function(event) {
  // C'est là que la magie opère, Noël !
});
```

`event.respondWith` permet d'enregistrer une [réponse personnalisée](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent/respondWith) ou de gérer les erreurs du réseau.

Ensuite on retourne simplement la ressource qui correspond à la requête si elle est disponible dans le cache :

```javascript
caches.match(event.request);
```

S'il n'y a pas de correspondance dans le cache, la promesse `caches.match` sera rejetée et nous aurons une erreur réseau classique ([404](https://en.wikipedia.org/wiki/HTTP_404)). Mais on peut aussi utiliser les possibilités des Service Workers pour proposer un traitement plus adéquat à nos erreurs réseau :

```javascript
caches.match(event.request).catch(function() {
  return fetch(event.request);
})
```

Ici, si le cache ne contient pas la ressource correspondante à la requête, on tente de faire un appel réseau avec cette même requête. Et dans le cas de notre exemple, nous faisons encore mieux en rajoutant cette ressource au cache si la requête réseau a fonctionné :

```javascript
var response;
event.respondWith(caches.match(event.request).catch(function() {
  return fetch(event.request);
}).then(function(r) {
  response = r;
  caches.open('v1').then(function(cache) {
    cache.put(event.request, response);
  });
  return response.clone();
}));
```

_Nous retournons une copie de la réponse car les objets requêtes et réponses sont des flux qui ne peuvent être consommés qu'une seule fois._ Ici on consomme la réponse pour l'ajouter dans le cache et on retourne la réponse à celui qui a fait la requête, l'objet `XMLHttpRequest` de `app.js`.

Enfin, si la requête n'a pas de correspondance dans le cache et si le réseau n'est pas disponible, on tombe dans le dernier cas :

```javascript
catch(function() {
  return caches.match('/sw-test/gallery/myLittleVader.jpg');
})
```

Et on retourne une ressource _par défaut_, ici une des images que nous savons disponibles dans le cache.

Pour preuve, une fois le Service Worker activé (visitez une première fois https://mdn.github.io/sw-test), essayez donc de requêter la ressource [https://mdn.github.io/sw-test/mylittleponey](https://mdn.github.io/sw-test/mylittleponey) qui n'existe pas dans l'application de démonstration.

## Mise à jour et gestion du cache

Lorsque nous avons déjà une version de notre Service Worker installée et activée, celle-ci restera responsable des traitements des requêtes tant qu'il y aura des pages utilisant cette version du Service Worker.

Si nous mettons à disposition une nouvelle version de notre Service Worker, celui-ci sera installé en arrière-plan, mais ne sera activé et responsable des traitements de requêtes que lorsque plus aucune page chargée n'utilisera l'ancienne version.

Pour mettre à jour notre Service Worker, il nous suffira de changer la version du cache comme ceci :

```javascript
this.addEventListener('install', function(event) {
  event.waitUntil(
    caches.open('v2').then(function(cache) {
      return cache.addAll([
        '/sw-test/',
        '/sw-test/index.html',
        '/sw-test/style.css',
        '/sw-test/app.js',
        '/sw-test/image-list.js',

             …

              // include other new resources for the new version...
      ]);
    });
  );
});
```

L'événement __activate__ est émis juste avant que le Service Worker ne soit responsable des traitements des requêtes, c'est donc le bon moment pour gérer le cache en supprimant les entrées qui ne sont plus nécessaires, par exemple.

On écrira quelque chose comme ça :

```javascript
this.addEventListener('activate', function(event) {
  var cacheWhitelist = ['v2'];

  event.waitUntil(
    caches.keys().then(function(keyList) {
      return Promise.all(keyList.map(function(key) {
        if (cacheWhitelist.indexOf(key) === -1) {
          return caches.delete(keyList[i]);
        }
      }));
    })
  );
});
```

## Dev tools

Les navigateurs qui implémentent le nécessaire aux Service Workers ajoutent quelques outils pour les lister, les debugger, les démarrer, les stopper, les désinscrire (__unregister__).

Pour Chromium, les outils sont disponibles via [`chrome://inspect/#service-workers`](chrome://inspect/#service-workers) et [`chrome://serviceworker-internals`](chrome://serviceworker-internals).

Pour Firefox, ils sont disponibles via [`about:serviceworkers`](about:serviceworkers).

Les dev tools vous permettent également de tester les mode offline de votre web app comme vu plus haut avec : pour [Firefox](https://support.mozilla.org/en-US/questions/895486) ou pour [Chromium](http://stackoverflow.com/questions/16091243/does-chrome-have-a-work-offline-option).

## Annexes

Enfin voici quelques ressources pour aller plus loin.

- Introduction to Service Worker, [Matt Gaunt](https://twitter.com/gauntface) : [http://www.html5rocks.com/en/tutorials/service-worker/introduction/](http://www.html5rocks.com/en/tutorials/service-worker/introduction/)
- Service Worker Cookbook, [Mozilla](https://www.mozilla.org/en-US/) : [https://serviceworke.rs/](https://serviceworke.rs/)
- Offline Recipes for Service Workers, [David Walsh](https://twitter.com/davidwalshblog) : [https://hacks.mozilla.org/2015/11/offline-service-workers/](https://hacks.mozilla.org/2015/11/offline-service-workers/)
- Service Worker API, [Mozilla Developer Network](https://developer.mozilla.org/en-US/) : [https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- Using Service Workers, [MDN](https://developer.mozilla.org/en-US/) : [https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers)
- Service Worker test repository, [MDN](https://developer.mozilla.org/en-US/) : [https://github.com/mdn/sw-test](https://github.com/mdn/sw-test)
- Can I Use Service Workers?, [Alexis Deveria](http://twitter.com/Fyrd) : [http://caniuse.com/#search=service%20worker](http://caniuse.com/#search=service%20worker)
- Getting started with Service Workers, [Ritesh Kumar](https://twitter.com/ritz078) : [http://www.sitepoint.com/getting-started-with-service-workers/](http://www.sitepoint.com/getting-started-with-service-workers/)
- Is Service Worker ready?, [Jake Archibald](https://twitter.com/jaffathecake) : [https://jakearchibald.github.io/isserviceworkerready/resources.html](https://jakearchibald.github.io/isserviceworkerready/resources.html)
- Une nouvelle architecture pour nos applications web mobiles, [Julien Wajsberg](https://twitter.com/jwajsberg) : [http://www.24joursdeweb.fr/2015/une-nouvelle-architecture-pour-nos-applications-web-mobiles/](http://www.24joursdeweb.fr/2015/une-nouvelle-architecture-pour-nos-applications-web-mobiles/)
- Background Sync, [Web Incubator Community Group](https://wicg.github.io/admin/charter.html) : [https://github.com/WICG/BackgroundSync/blob/master/explainer.md](https://github.com/WICG/BackgroundSync/blob/master/explainer.md)
- Cache, [MDN](https://developer.mozilla.org/en-US/) : [https://developer.mozilla.org/en-US/docs/Web/API/Cache](https://developer.mozilla.org/en-US/docs/Web/API/Cache)
- FetchEvent, [MDN](https://developer.mozilla.org/en-US/) : [https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent)
- postMessage, [MDN](https://developer.mozilla.org/en-US/) : [https://developer.mozilla.org/en-US/docs/Web/API/Client/postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Client/postMessage)
- Using Service Workers, [MDN](https://developer.mozilla.org/en-US/) : [https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers)
