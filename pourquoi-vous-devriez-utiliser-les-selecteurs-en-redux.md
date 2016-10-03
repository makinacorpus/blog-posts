# Pourquoi vous devriez utiliser les sélecteurs en Redux

Lorsque l'on fait des applications en React, on atteint parfois le point où, l'état de notre application grossissant, nous prenons la décision de passer à Redux.

## Pourquoi Redux et les sélecteurs ?
Basé sur l'architecture Flux, la librairie de Dan Abrmhov s'est imposée comme la référence de la communauté dès qu'il s'agit de mettre en place une gestion d'état partagée.
Cela permet à la fois de se protéger des problèmes de concurrence sur la lecture et l'écriture de l'état de votre application,
mais surtout cela offre une séparation des responsabilités très importantes dès lors que votre application grandit.
Votre composant *Container* peut alors aller lire l'état global (*state*) de votre application et fournir aux composants *Presentationals* les données dont ils ont besoins.

Et c'est là que se trouve notre problème. Souvent le state de l'application ne se présente pas exactement sous le modèle que les composants *Presentationals* attendent.
On crée donc dans notre composant *Container* des fonctions pour manipuler le state et renvoyer juste ce qu'il faut.
Et là nous venons de briser la règle de séparation des responsabilités.
Le *Container*, en plus de faire la connexion avec le *state* Redux commence à manipuler ce dernier (même s'il ne manipule pas dirèctement le *state* mais la copie qu'il reçoit).
C'est là que rentrent en jeux les *selectors*.

## Qu'est-ce qu'un selecteur ?
Un *selector* est tout simplement une **fonction qui prend en paramètre tout ou partie du state de l'application et en renvoit une version formatée et/ou réduite, propice aux besoins de nos vues**.
Concrètement un selector très simple pourrait être :
```
const getSubstate = state => state.subState;
```
Ce *selector* sera alors exporté par le fichier où il se trouve, puis appelé par les composants *Container* ayant besoin de la donnée formatée.
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
Si on pourrait croire que l'on ne fait que déplacer notre code, on s'assure surtout que notre modèle n'est manipulé que par Redux,
mais surtout que nos composants *Containers* ne sont responsables que de **gérer la logique d'interface**.
Cela nous permet d'avoir des composants petits et maintenables facilement, peu importe leur type.

## Pour résumer
Un *selector* est donc :
* une fonction prenant en paramètre au moins le *state* de l'application
* situé dans le même fichier que *reducers* correspondant
* combinable avec d'autres *selectors*
* un moyen d'accéder à des sousparties du *state* de l'application
* un moyen de reformater des parties du *state* à la volée pour les vues
* un moyen de conserver un *state* global simple et sans répétition