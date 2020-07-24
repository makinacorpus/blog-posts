image de l'article : quelquechose comme https://weknowinc.com/static/8fd2939f17380d172acd361e32f0bab9/f2cbb/gatsby-loves-drupal.jpg

[Gatsby](https://www.gatsbyjs.org/) est une solution permettant de créer simplement des sites statiques à partir de React.
Gatsby permet entre autre d'intégrer du contenu à un site statique à partir de *sources de données*. Nous allons nous voir
dans cet article dans quelle mesure il est possible de se service d'un Drupal pour jouer le rôle de cette source de données.

L'interêt d'une telle architecture est multiple :

* On profite de la puissance de Drupal :
  * On définie des types de contenus et organise ces contenus autour de taxonomies et menus
  * On bénéficie de son backoffice pour contribuer facilement (workflow, gestion des médias..)
* Mais aussi de la puissance de Gatsby :
  * Le site statique généré est très rapide
  * On améliore grandement la sécurité : l'accès au backoffice Drupal est protégé, le visiteur lamba n'a accès qu'à des fichiers statiques

Il existe un [plugin officiel](https://www.gatsbyjs.org/docs/sourcing-from-drupal/) pour récupérer des contenus depuis Drupal.
Ce module permet de requêter en GraphQL les données exposés par le module *JSON:API* de Drupal (et par *JSON:API extra* qui
est fortement conseillé).

On peut déjà faire quelques remarques :

* Un site construit avec Gatsby hérite automatiquement des limitations de *JSON:API*
* *JSON:API* est, depuis peu, inclus dans le core de Drupal : c'est donc plutôt une solution pérenne
* *JSON:API extra* est dors et déjà disponible sur Drupal 9, il n'y a pas de soucis à ce faire pour le future

Pour cette expérimentation, j'ai repris [l'exemple fournit pas Gatsby](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-drupal)
qui utilise [le site démo Umami](https://www.drupal.org/project/umami) de Drupal.

# Accéder aux contenus de Drupal

## Les Nœuds et Taxonomies :

L'exemple fournit est très simple (presque trop ?), il ne fait que requêter des nœuds, mais ça marche bien.
Le GraphQL est pratique : une interface (GraphiQL) est disponible pour construire/tester ses requêtes, on peut découvrir les différents types
de contenu et leurs différents champs. Le développeur front n'a pas besoin de maitriser Drupal, l'API est autodocumenté.

image: aperçu de graphiQL

Il n'y a pas, ici, de limitation particulière, les taxonomies peuvent être exploité de la même manière, aucune difficulté en vue.

## Les Menus :

Je disais au paragraphe précédent que l'exemple fournit était presque trop simple, il n'essaye pas, par exemple, de récupérer des menus : ils sont
renseignés en dur dans les templates React. Les menus sont une fonctionnalité essentielle pour rendre les contributeurs un minimum
autonomes, j'ai donc commencé par essayer de voir comment les atteindre.

Mauvaise surprise : ce n'est pas si évident ! Disons-le, ce n'est pas vraiment la faute de Gatsby, ni celle de *JSON:API*, mais plutôt de Drupal.

En fait, pour récupérer les liens de menus via *JSON:API*, il faut donner les droits *Administration des menus* aux utilisateurs anonymes. C'est
loin d'être anodin - ce droit donne par exmeple la possiblité de créer/modifier/supprimer les menus du site - et ce n'est donc absolument pas
souhaitable ! La communauté travaille activement sur [ce problème](https://www.drupal.org/project/drupal/issues/2915792) et un patch est déjà disponible.

Une fois que l'on a accès à ces liens de menu depuis Gatsby, on se rend compte qu'il n'y a pas de moyen simple de les obtenir rangés dans une belle
arborescence, il faut la reconstruire soi-même. Un module React semble proposer de le faire pour nous : [gatsby-druapl-menus](https://www.npmjs.com/package/@xaviemirmon/gatsby-drupal-menus),
mais je ne l'ai pas encore tester.

On voit ici qu'il n'est pas encore simple, pour un Drupal découplé, de tirer profit des menus. On finit par y arriver, mais il faudra compter sur un
peu de développement.

## Les Blocs :

Je n'avais jamais encore vraiment testé la réalisation d'un Drupal découplé. Ce qui est perturbant est qu'il faut garder à l'esprit que tout ce qui
concerne le thème dans Drupal n'est, de fait, plus disponible. On peut, par exemple, considérer que tout ce qui ce trouve dans un fichier *.theme* ne
sera pas disponible du côté Gatsby. Il faut aussi dire au revoir aux régions et au placement automatique des blocs !

Il faudra donc ruser pour replacer correctement ses blocs sur les différentes pages, on pourra se référer à
[cet exemple bien documenté](https://www.jamesdflynn.com/development/using-drupal-blocks-decoupled-gatsbyjs-application). Le plus pertinent est à mon
avis d'*oublier* complètement le système de thème de Drupal, et de réfléchir de manière radicalement différente pour réaliser la disposition des
différents éléments du site.

Les blocs sont accessibles via *JSON:API*, mais on ne récupère pas pas le bloc rendu - on vous a dit qu'il fallait oublier le thème, donc aucun rendu
n'est réalisé ! - mais seulement les valeurs des différents champs. Ça limite donc l'utilisation d'un certain nombre de blocs. Mais, par exemple,
on pourra utiliser tout les blocs personnalisés déclarés via la backoffice.

Mon avis est mitigé sur le cas des blocs, on perd la puissance du thème Drupal pour leur placement : il faudra une certaine dose de réflexion
pour repenser les différents cas d'utilisation. Mais après tout, si on souhaite découpler son site, c'est bien pour s'affranchir de Drupal, non ?

## Les Vues :

Je sais que certains sont très adeptes des vues, et bien désolé, mais à priori, il n'est pas possible de les utiliser facilement avec Gatsby. Cependant,
au vu des possibilités offertes par le GraphQL pour faire des requêtes (conditions sur les champs des contenus, pagination...) je ne pense pas que
 Views soit indispensable pour réaliser un frontoffice.

L'utilisation est donc à priori compliqué, mais finalement, se passer de Views est-ce vraiment *si* grave ? ;)

# Le cycle de publication

A la différence d'un Drupal classique, ici, l'étape de génération du site statique rend la publication de contenu *pseudo-temps réel*.

L'arrivée d'un contenu jusqu'au visiteur final se fera en 2 grandes étapes :

* Les contributeurs contribuent via le BO de Drupal sur un serveur à l'accès restreint
* Le site statique est regénéré par Gatsby à partir des données contribuées, puis déployé sur le serveur de production

## La contribution

Le contributeur ne verra pas de différences avec un Drupal classique, il ne sera pas perdu, et pour cause : son interface *est* le backoffice
classique de Drupal.

La seule question qui peut poser problème est l'aperçu du rendu final du contenu. C'est une fonctionnalité quasi-indispensable, on peut difficilement
imaginer un contributeur remplir un WYSIWYG, quelques champs et prier pour que le rendu soit comme il l'imaginait une fois qu'il est publié ! Dans notre
cas le rendu du contenu sera fait par React qui n'est *à priori* pas disponible côté Drupal. Et après tout, c'est l'idée du découplage. Heureusement, un
module Drupal permet de reproduire le contenu tel qu'il sera publié : [Gatsby Live Preview](https://www.drupal.org/project/gatsby). Ce module n'est par
contre pas encore disponible pour D9 et est en version alpha sur D8, je n'ai pas eu le temps de le tester.

image :
Aperçu de Gatsby Live Preview
https://www.drupal.org/files/project-images/media%20preview%20working.gif

## La génération du site statique

Le site étant statique, à la différence d'un Drupal classique, l'appuie sur le bouton 'Publier' d'un contenu n'aura pas pour effet de le rendre immédiatement
disponible au public. Pour ça, il faudra d'abord le regénérer (c'est le boulot de Gatsby).

On a 3 possibilités pour déclencher cette génération :

* Périodiquement
* Manuellement, via le BO de Drupal ou une autre interface
* Sur certaines actions (publication/dépublication, modification de menu...)

Les deux premières options ne posent pas de problème. La troisième par contre implique de créer d'un lien entre Drupal et Gatsby. La bonne
nouvelle est que c'est une des fonctionnalités du module Gatsy Live Preview. Ça semble être uniquement pour la publication de contenu, mais
moyennement l'implémentation de deux ou trois hook, j'imagine qu'il ne sera pas trop compliqué de généraliser ce comportement à d'autres actions
(modification de taxonomie, création d'un lien de menu...)

Avant que la génération du site deviennent lente il faudra avoir atteint un certain volume de contenus. Mais lorsque cela arrivera, sachez que
Gatsby fournit la possibilité de fairte de la génération incrémentale, ce qui devrait alléger considérablement la regénération du site.

mettre en place ce genre d'architecture ne devrait pas être trop compliqué. La plus grande diffuculté sera sans la pédagogie à faire au près des
contributeurs pour leur expliquer le léger temps de latence entre la publication du contenu côté Drupal et son apparition effective sur le site :
c'est là l'un des seuls points faibles pour ce type de solution.

# Pour conclure

Une fois le problème de menu résolu, je pense que tout est aujourd'hui réuni pour faire un site Drupal découplé et le couplage avec Gatsby semble très
prometteur.

En résumé avec le couple Drupal/Gatsby :
* Ce que l'on peut faire facilement : la récupération des nœuds/taxonomies/blocs/médias via des jolies requêtes GraphQL
* Ce qui demande un peu de contournement : les accès aux menus et le placement de blocs
* Ce qu'il n'est pas possible de faire : l'utilisation de vues
* Ce qui peut être frustrant : le temps entre la publication côté Drupal et l'apparition du contenu sur le site

C'était une des premières remarques de l'articles, et on le voit ici, la plus grosse limitation n'est pas du fait de Gatsby, mais bien des possibilités offertes
par Drupal et sa *JSON:API*. L'utilisation d'un Drupal headless couplé avec une application JS ayant le vent en poupe, cette limitation sera rapidement
être levées : c'est notament sur [la feuille de route de Drupal 10](https://www.drupal.org/blog/state-of-drupal-presentation-july-2020) et de son futur
 *Javascript menu component* !

image :
    'Planting the flag' for providing official JavaScript menu components for Drupal.
    https://www.drupal.org/files/drupal-10-javascript-menu-component-1-1280w.png


