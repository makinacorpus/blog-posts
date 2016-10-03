# Pourquoi vous devriez utiliser les sélecteurs avec Redux

Lorsque l'on fait des applications avec React, on atteint parfois le point où, l'état de notre application grossissant, on prend la décision d'ajouter [Redux](http://redux.js.org/).

## Pourquoi Redux et les sélecteurs ?
Basée sur [l'architecture Flux](https://facebook.github.io/flux/docs/overview.html#structure-and-data-flow), la librairie de [Dan Abramov](https://twitter.com/dan_abramov) s'est imposée comme l'une des références pour la communauté dès qu'il s'agit de mettre en place une gestion d'état partagée.
Cela permet de se protéger à la fois des problèmes de concurrence sur la lecture et l'écriture de l'état de votre application,
mais surtout cela offre une séparation des responsabilités très importante dès lors que votre application grossit.
Votre composant *Container* peut alors aller lire l'état global (*state*) de votre application et fournir aux composants *Presentational* les données dont ils ont besoin. (Si vous ne connaissez pas la différence, courez lire ce [superbe article](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.ueuyblexk) !)

Et c'est là que se trouve notre problème. Souvent l'état de l'application ne se présente pas exactement sous le modèle que les composants *Presentational* attendent.
On crée donc dans notre composant *Container* des fonctions pour manipuler le state et renvoyer juste ce qu'il faut.

Et là nous venons de briser la règle de [séparation des responsabilités](https://en.wikipedia.org/wiki/Separation_of_concerns).
Le *Container*, en plus de faire la connexion avec le *state* Redux commence à manipuler ce dernier (même s'il ne manipule pas directement le *state* mais la copie qu'il reçoit).
C'est là que rentrent en jeux les *selectors*.

## Qu'est-ce qu'un sélecteur ?
Un *selector* est tout simplement une **fonction qui prend en paramètre tout ou partie du state de l'application et en renvoie une version formatée et/ou réduite, propice aux besoins de nos vues**.
Concrètement un selector très simple pourrait être :

```
const getSubState = state => state.subState;
```

Ce *selector* sera écrit dans le fichier du reducer et exporté, puis importé par les composants *Container* ayant besoin de la donnée formatée.
Par exemple :

```
...
import { getSubstate } from '../reducers/my-reducer';
...

const mapStateToProps = (state) => {
  return {
    dataNeeded: getSubstate(state)
  }
}
```

Si on peut croire que l'on ne fait que déplacer notre code, on s'assure surtout que notre modèle n'est manipulé que par Redux,
ainsi nos composants *Containers* ne sont responsables que de **gérer la logique d'interface**.
Cela nous permet d'avoir des composants petits et facilement maintenables, peu importe leur type.

## Combiner des sélecteurs
Au même titre que n'importe quelle fonction, un sélecteur doit être simple, pur et responsable d'une seule opération.
Il devient alors possible d'imaginer des combinaisons de sélecteurs afin d'obtenir le modèle souhaité.
Par exemple :

```
const getSubState = state => state.subState;
const reduceState = state => state.map(myMapingFunction);
const getNeededData = state => reduceState(getSubState(state));
```

L'exemple est ici un peu simple mais illustre assez bien l'idée.
De plus, cela se rapproche pas mal de la programmation fonctionelle qu'implémente Redux.

## Pour résumer
Un *selector* est donc :

* une fonction prenant en paramètre au moins le *state* de l'application
* une fonction renvoyant un modèle spécifique à un besoin
* situé dans le même fichier que le *reducer* correspondant
* combinable avec d'autres *selectors*
* un moyen d'accéder à des sous-parties du *state* de l'application
* un moyen de reformater des parties du *state* à la volée pour les vues
* un moyen de conserver un *state* global simple et sans répétition

## Pour aller plus loins
Si vous souhaitez aller plus loin avec les *selectors*, la librairie [Reselect](https://github.com/reactjs/reselect) semble être le choix du moment. Elle vous permettra notamment de combiner des sélecteurs très simplement ou d'utiliser la *memoization* pour éviter de recalculer les résultats d'un sélecteur à chaque appel.
Et si vous vous sentez un peu perdu, n'hésitez pas à regarder du coté de [notre formation React](https://makina-corpus.com/formations/formation-react) !
