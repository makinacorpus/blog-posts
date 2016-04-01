# Les besoins spécifiques de l'app mobile

Chez Makina Corpus, nous faisons des [applications mobiles hybrides](http://edit.makina-corpus.com/blog/metier/2016/quelle-solution-pour-mon-application-mobile-hybride).
Il n'est pas rare que nos clients maitrisent leur métier et qu'ils fournissent directement une interface pour accéder aux données.
L'application mobile consommera donc une API (*Application Programming Interface*) servie par un serveur web.
Le design de cette API est très important pour construire quelques choses qui conviendra aux développeurs :
une API web peut être plus ou moins pratique, on pourra parler alors de *DX*, l'expérience développeur.
Comme pour l'expérience utilisateur (*UX*), l'important peut être articulé autour des 3 U :

1. Utilisable
2. Utilisée
3. Utile

## Les ressources

L'API doit servir des ressources ;
les ressources métier qui sont nécessaires aux différentes applications clientes qui la consommeront.
Il faut bien repérer les ressources métier :
ce n'est pas évident pour le développeur de l'application client qui ne connait pas forcément le métier de faire le point sur une API qui n'est pas bien écrite.
En effet, le développeur de l'application qui consomme l'API n'a pas besoin d'avoir une connaissance poussée du métier, à l'instar de l'utilisateur.

Prenons l'exemple d'une application comme gitlab, un serveur pour gérer et héberger les projets git de votre équipe.
Une des ressources facilement repérables est le projet et nous retrouverons donc deux routes autour de cette ressource :

- `/projects` pour [récupérer l'ensemble des projets](http://doc.gitlab.com/ce/api/projects.html#list-projects) auxquels le token de la requête donne accès ;
- `/projects/{project_id}` pour [récupérer un project particulier](http://doc.gitlab.com/ce/api/projects.html#get-single-project) à partir de son identifiant.

### Les filtres et recherches

Autour de ces deux routes, nous pourrons ajouter des query parameters qui vont permettre de donner plus de souplesse lors de nos appels en fonction de nos besoins spécifiques :
**filtrer**, **ordonner** ou **chercher** sur cette ressource.
On pourra par exemple appeler :

- `/projects?order_by=created_at` qui permet d'ordonner la réponse par date de création ;
- `/projects?search=ionic` qui retourne la liste des projets auxquels l'appelant à accès et qui contiennent le terme *ionic*.

On pourrait également imaginer utiliser les *query parameters* pour limiter les champs retournés par l'API (un peu à la manière de ce que propose la sépcification GraphQL) :

- `/projects?fields=ssh_url_to_repo,owner,name` pour récupérer le nom du projet, son responsable et un lien.

### Les relations

Les relations devraient être des sous-ressources car il n'y a aucune raison que l'application cliente refasse les jointures de son côté.
Ainsi on aura différentes routes permettant de récupérer les ressources relatives à un projet :

- `/projects/{project_id}/members` pour [les membres d'un projet](http://doc.gitlab.com/ce/api/projects.html#list-project-team-members) ;
- `/projects/{project_id}/repository/branches` pour [les branches du dépôt d'un projet](http://doc.gitlab.com/ce/api/projects.html#list-branches).

Là encore c'est grâce au token qu'on reconnait l'utilisateur qui en fait la demande et qu'on adapte la réponse en ne renvoyant que les données auxquelles l'appelant a le droit d'accéder.
C'est pour ça qu'on parle d'API *stateless*, l'état n'est pas conservé par le serveur :
chaque requête contient l'ensemble des éléments permettant de répondre.

De la même manière, on peut récupérer une sous-ressource particulière grâce à sa clef, son identifiant :

- `/patients/{project_id}/members/{user_id}` pour [accéder aux données d'un membre particulier du projet](http://doc.gitlab.com/ce/api/projects.html#get-project-team-member).

Il n'y a aucun problème à délivrer les mêmes ressources via plusieurs routes si cela a du sens.
Prenons les membres d'un projet par exemple.
Une user story pourrait être :
*en tant qu'administrateur je veux pouvoir administrer les informations des utilisateurs afin de modifier leurs données*.
Et une autre :
*en tant qu'utilisateur, je veux pouvoir voir les informations des membres d'une équipe d'un projet*.
On aura donc plusieurs routes qui délivrent la même ressource, mais pas forcément de la même manière :

- `/users/{user_id}` permet de [récupérer l'ensemble des données pour un utilisateur](http://doc.gitlab.com/ce/api/users.html#for-user) ;
- `/projects/{project_id}/members/{user_id}` permet de [récupérer les champs *name*, *username*, *id*, *state*, *avatar_url* et *access_level*](http://doc.gitlab.com/ce/api/projects.html#get-project-team-member).

En toute logique, pour un même `user_id`, les informations renvoyées sont les mêmes quelles que soit la route.
Les champs seront différents si cela a une raison. Inutile de récupérer les champs *website_url* d'un utilisateur pour afficher la liste des membres d'une équipe d'un projet.
Par contre on pourra retrouver cette information dans l'application cliente en créant un lien, grâce à l'identifiant, vers la ressource principale.

## Les verbes HTTP

Pour chacune des ressources, on utilisera les verbes HTTP pour faire des actions précises :

- sur les ressources globales (les noms aux pluriels, ex : `/projects`) :
    - `GET` pour récupérer la liste des éléments ;
    - `POST` pour créer un nouvel élément.
- sur les ressources particulières (les routes avec un identifiant, ex : `/projects/338`) :
    - `GET` pour récupérer les informations de cet élément ;
    - `PUT` ou `PATCH` pour modifier les informations de cet élément ;
    - `DELETE` pour supprimer cet élément.

## Offline

Pour certaines applications mobiles, nous devons permettre à l'utilisateur de faire des actions même s'il n'a pas de connexion internet.
Il n'est pas envisageable de monter une base de données synchronisée avec le serveur sur chacun des téléphones ou tablettes.
Il n'est pas encore forcément possible d'utiliser les caches d'un [Service Worker](https://edit.makina-corpus.com/blog/metier/2016/decouvrir-le-service-worker) sur une application mobile hybride.

L'une des solutions possibles est donc de charger suffisamment d'informations depuis le serveur lorsqu'une connexion est disponible.
En reprenant notre exemple gitlab, on pourra donc avoir une route `/projects` avec des *query parameters* particulier comme `/projects?fields=all` qui nous renverra l'ensemble des données des projets.
Ainsi nous naviguerons dans notre application avec les données des projets nécessaires et suffisantes.
Les données renvoyées dans un tel cas sera un gros tableau json contenant les objets *projects* qui contriendront toute une structure de données arborescente permettant à l'application d'accéder aux sous-ressources d'un projet.
Pour récupérer tous les commits des projets auxquels on a accès, pour faire un page d'accueil contenant l'activité récente, on pourra faire quelque chose comme ça :

``` javascript
var commits = projects.map(function(project) {
  return project.commits;
});
```
Ou encore, pour afficher le nombre d'événements pour l'ensemble des projets ou par projet, on pourra faire quelque chose comme :

```javascript
var totalEvents = projects.reduce(function(count, project) {
  return count + project.events.length;
}, 0);
var events = projects.map(function(project) {
  return {
    name: project.name,
    count: project.events.length
  };
});
```

Pour l'afficher dans notre template [ionic](http://edit.makina-corpus.com/blog/metier/2015/bien-demarrer-avec-ionic)/angular de la sorte :

```html
<div class="notification">
  <span class="notification__badge">{{totalEvents}}</span>
</div>

<ul>
  <li class="notification" ng-repeat="event in events">
    {{event.name}}
    <span class="notification__badge">{{event.count}}</span>
  </li>
</ul>
```

Sur une web app en ligne, nous appellerons les ressources quand nous en avons besoin, en acceptant qu'elle renvoie des informations complètes et auto-descriptives.
Inutile de renvoyer une ressource en mode base de données relationnelle de ce type :

``` json
{
  "id": 12,
  "author_id": 13,
  "category": 0,
  "root": true
}
```

Les données renvoyées par l'API doivent être **explicites** et **complètes**.
Les ressources doivent être logiquement organisées en fonction du  métier, pas en fonction de la logique ORM (*Object-Relational Mapping*) ou de la base de données relationnelle utilisée.
Les ressources ne doivent pas non plus être organisées en fonction de l'application tierce qui consommerait ces dernières.
Il est toujours possible d'ajouter des *query parameters* pour spécifier une ressource en fonction d'un besoin particulier.

Les ressources doivent être des noms plutôt que des verbes.
On écrira la route `/projects` plutôt que `/getProjects`.
Les verbes seront réservés aux actions comme `/search` pour effectuer une [recherche globale comme dans l'API de twitter](https://dev.twitter.com/rest/public/search).

Même si `/projects` renvoie tout ce qui est nécessaire pour un mode hors ligne, il faudra quand même construire les routes spécifiques aux ressources et sous-ressources.
La requête `POST /projects/{project_id}/members` permettra d'[ajouter un membre à un projet](http://doc.gitlab.com/ce/api/projects.html#add-project-team-member).
La requête `PUT /projects/{project_id}/hooks/{hook_id}` permettra de [modifier un hook pour un projet spécifique](http://doc.gitlab.com/ce/api/projects.html#edit-project-hook).

Dans le cadre d'une application hors ligne, on prendra soin de conserver ces requêtes de *modification* dans une pile qu'on videra lorsque la connexion sera disponible.

## Pour aller plus loin

Vous pouvez retrouver tout un tas de très bons conseils sur le blog de [Vinay Sahni](http://twitter.com/veesahni) qui me sert souvent de référence : [Best Practices for Designing a Pragmatic RESTful API](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api).

Dans une version plus complète, [le livre blanc](https://apigee.com/about/resources/ebooks/web-api-design) de [apigee](https://apigee.com/about/) est aussi une très bonne référence : [Web API Design: Crafting Interface that Developers Love (PDF)](https://pages.apigee.com/rs/351-WXY-166/images/ebook-2013-03-wad.pdf).

Une bonne documentation pour votre API est souvent essentielle, quand je dois en faire, je la réalise en pseudo markdown avec la spécification [opensource](https://github.com/apiaryio/api-blueprint) [blueprint](https://apiblueprint.org/) ou encore avec la spécification yaml de l'[Open API Initiative](https://openapis.org/specification) basée sur [swagger](http://swagger.io/), lui aussi [opensource](https://github.com/swagger-api).
La documentation via ces deux spécifications permettent de générer une page html ainsi qu'un [serveur mock](https://apiblueprint.org/tools.html).

Enfin si vous avez besoin de tester votre API, rien de tel qu'un petit client REST comme [\\</\\> Rested](https://github.com/esphen/RESTED).

Autre proposition : allez voir les API des gros services comme celle de [gitlab](http://doc.gitlab.com/ce/api/), [github](https://developer.github.com/v3/), [twitter](https://dev.twitter.com/rest/public), [enchant](http://dev.enchant.com/api/v1), etc.


[Illustration: [Peter Miller CC BY-NC-ND 2.0](https://flic.kr/p/quxQih)]
