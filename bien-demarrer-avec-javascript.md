# Bien démarrer avec javascript

Javascript a mauvaise presse auprès des développeurs des autres langages. J'ai vu de nombreux développeurs râler quand ils ont du en écire quelques instructions. Il faut bien avouer que [certains comportements ne sont pas intuitifs](https://www.destroyallsoftware.com/talks/wat). Cependant, le constat est clair : lire la doc du langage permet de comprendre bien des choses dans ce langage qui anime toutes nos pages web.

## Les deux mondes

Le premier des deux mondes apparut en 1995 grâce à [Brendan Eich](https://twitter.com/BrendanEich), qui travaillait chez Netscape. À cette période et jusqu'à maintenant, [javascript sert à animer des pages web](https://en.wikipedia.org/wiki/JavaScript) en proposant des apis et la gestion de l'arbre DOM dans le navigateur du visiteur.

http://instalaya.com/wp-content/uploads/2011/04/netscape-3.bmp

Le deuxième monde est [apparut en 2009](https://en.wikipedia.org/wiki/Node.js) grâce à [Ryan Dahl](https://www.youtube.com/watch?v=jo_B4LTHi3I) qui se dit alors que le moteur V8 de Google est suffisamment rapide pour faire tourner du code côté serveur. Il exploite donc le moteur et y ajoute quelques api, notamment une api d'entrée/sortie non bloquantes et des modules.

Depuis 2009, javascript est donc un langage permettant d'exécuter des instructions côté front dans le navigateur du visiteur, ou côté back sur le serveur des fournisseurs de services. Ces deux mondes sont très proches, mais on leurs particularités comme nous le verrons dans la suite.

## this and the scope

Un des pièges courament rencontré par les développeurs qui veulent écrire du javascript sans en comprendre les concepts provient du _scope_. Le scope est ce qu'on appelle en français [la portée](https://fr.wikipedia.org/wiki/Port%C3%A9e_%28informatique%29), c'est à dire la visibilité accordée à une variable. Le scope porte également le contexte dans lequel les instructions vont s'exécuter.

### le scope global

Dans les deux mondes, le scope global est différent. Mais le concept est le même. Si une déclaration est globale, sa valeur sera accessible partout. Pour le navigateur, le scope global est l'objet `window`. En fait, si nous chargeons un fichier javascript dans une page html avec [la seule déclaration suivante](http://jsbin.com/lajawevezu/2/edit?html,js,console) :

```
var sideEffect = "Hello World!";

function runInLocalScope() {
  console.log(sideEffect); //Hello World!
}

runInLocalScope();
```

Tout se passe comme si on écrivait [ceci](http://jsbin.com/lajawevezu/3/edit?html,js,console) :

```
window.sideEffect = "Hello World!";

window.runInLocalScope = function() {
  console.log(window.sideEffect);
};

window.runInLocalScope(); //Hello World!

// d'ailleurs

console.log(window.runInLocalScope); //function()...
}
```

Nous voyons tout de suite ce qui peut [poser souci](http://jsbin.com/dobibakoku/1/edit?html,js,console), j'ai laissé un indice en nommant notre variable :

```
var sideEffect = "Hello World!";

function runInLocalScope() {
  console.log(sideEffect);
}

function doSideEffect() {
  sideEffect = "Farewell and good night!";
}

runInLocalScope(); //Hello World!
doSideEffect();
runInLocalScope();//Farewell and good night!
```

Notre variable `sideEffect` n'est pas immutable, la fonction `doSideEffect` en a modifié son contenu, pour l'ensemble du scope global et donc quelque soit l'endroit de mon application dans lequel je vais appeler cette variable.

Il est donc recommandé de définir ses variables et ses fonctions dans des scopes locaux, afin d'eviter les [effets de bord](https://fr.wikipedia.org/wiki/Effet_de_bord_%28informatique%29) ce qui limitera les bugs. Quelques patterns sont à votre rescousse, comme la fonction anonyme auto-appelante (IIFE, [Immediately-Invoked Function Expression](http://benalman.com/news/2010/11/immediately-invoked-function-expression/)), comme [par exemple](http://jsbin.com/vimoquwaje/1/edit?html,js,console) :

```
var noSideEffect = "Hello World!";

function runInGlobalScope() {
  console.log(noSideEffect);
}

(function() {
  var noSideEffect = "Farewell and good night!";

  function runInLocalScope() {
    console.log(noSideEffect);
  }

  runInLocalScope(); //Farewell and good night!
  runInGlobalScope(); //Hello World!
})();

runInGlobalScope(); //Hello World!
```

### le scope local

En javascript, il n'existe que deux portées : le scope global et le scope local. Pour en faire le tour, voyons ensemble un effet amusant pour comprendre le scope local. Quelle sera l'affichage dans la console de cette suite d'instructions ?

```
var index;

for(index = 0 ; index < 5 ; index++) {
  setTimeout(function() {
    console.log("with side effect", index);
  }, 1000);
}

function recreateLocalScope(localIndex) {
  return function() {
      console.log("no side effect", localIndex);
  };
}

for(index = 0 ; index < 5 ; index++) {
  setTimeout(recreateLocalScope(index), 1000);
}
```

Même si nous nous trompons tous en essayant d'anticiper le résultat de ces instrutions, en [faisant tourner ce code](http://jsbin.com/wumadacupi/edit?html,js,console), on comprend vite ce qu'il se passe au sein des variables qui ne sont pas définies clairement dans un scope local.

### this

Dans de nombreux langages, [`this`](http://javascriptplayground.com/blog/2012/04/javascript-variable-scope-this/) est un pointeur vers l'objet courant. C'est un peu différent en javascript ; `this` est un pointeur vers le contexte qui exécute les instructions. Comme [le rappelle wikipedia](https://en.wikipedia.org/wiki/This_(computer_programming)#JavaScript), et de manière pragmatique, `this` peut se référer à :

- l'objet `window` s'il est utilisé dans un contexte de scope global ;
- l'objet portant la fonction qui est appelée, correspondant donc à un scope local.

Mais `this` dépend du contexte dans lequel la fonction est appelée. Si la fonction est [appelée via un événement comme le click de la souris](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#The_value_of_this_within_the_handler), alors `this` correspondra à ce contexte-ci, comme [nous pouvons le voir ici](http://jsbin.com/nayowodibu/edit?html,js,console,output) :

```
function displayThis() {
  console.log(this);
}

displayThis(); //[object window]

document.getElementById("button").addEventListener("click", displayThis); //[object HTMLButtonElement]
```

Il existe même deux fonctions pour changer le contexte de l'appel d'une fonction. [`bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) et [`call`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call). N'hésitez pas à vous référer aux exemples de MDN pour voir comment utiliser ces deux fonctions.

## asynchrone et l'enfer du callback

Le deuxième piège souvent rencontré par les développeurs javascript vient de la qualité de javascript a être un langage fonctionnel. C'est à dire que les fonctions sont des valeurs comme les autres :

```
var funfunvariable = function(name) {
  console.log("Hello " + name + "!");
}

function funfunfunction(name) {
  console.log("Hello " + name + "!");
}

var funfun = funfunfunction;
```

La seule différence entre ces deux déclarations est dans l'ordre d'évaluation de mise à disposition des fonctions dans le scope. L'instruction `function` permet à celle-ci d'être appelée avant sa déclaration. Alors que l'instruction sous forme de `variable` doit être interprétée avant tout appel à cette variable.

Une fois que nous avons compris cela, nous pouvons commencer à comprendre les callbacks. Que se passerait-il si je passais `funfunvariable` en tant que paramètre d'une autre fonction ?

```
function add(number1, number2) {
  return number1 + number2;
}

function multiply(number1, number2) {
  return number1 * number2;
}

function calculate(operation, number1, number2) {
  return operation(number1, number2);
}

console.log(calculate(add, 5, 7));
console.log(calculate(multiply, 5, 7));
```

En faisant la synthèse de tout ce que nous avons vu précédemment, on commence à entrevoir les points d'attention qu'il faudra porter à notre code javascript. Si nous passons une fonction en paramètre d'une autre fonction, dans quel scope nous trouvons nous ? Mais alors, que vaut `this` ?

```
var numbers = [5, 7];

function calculate(operation) {
  return operation(this.numbers[0], this.numbers[1]);
}

function add(number1, number2) {
  return number1 + number2;
}

console.log(calculate(add)); //12

(function() {
  var localScope = {
    numbers: [15, 21]
  };
  console.log(calculate.call(localScope, add)); //36
})();
```

Dans [cet exemple](http://jsbin.com/mevesuzace/edit?html,js,console), nous voyons comment modifier le contexte de la fonciton `calculate` avec la fonction `call`. Nous avons l'utilisation d'une fonction callback dans le paramètre de la fonction `calculate`. Et nous avons une utilisation de `this` qui peut être troublante.

Dans la vraie vie, les exemples d'utilisation courante que nous rencontrons pour les callbacks concernent plus volontiers les appels asynchrones, comme les appels HTTP à des ressources distantes. Autrement dit, nous passons à la fonction d'appel du json une fonction qui s'exécutera avec le json lorsque celui-ci sera renvoyé par le serveur :

```
function callGithubRepo(callback) {
  $.ajax({
    dataType: "json",
    url: 'https://api.github.com/repos/jquery/jquery/issues/1',
    success: callback
  });
}

function onSuccess(data) {
  console.log("The first issue on jquery is from: " + data.user.login);
  console.log("and its state is: " + data.state);
}

console.log("We will call the github api for the jquery repo...");
callGithubRepo(onSuccess);
console.log("The call is asynchronous...");
```

Dans [cet exemple](http://jsbin.com/tikurokuqe/1/edit?html,js,console), nous appelons l'api de github pour afficher la première issue sur le [repository de jquery](http://www.paulirish.com/2010/10-things-i-learned-from-the-jquery-source/) grâce à un appel ajax fait à l'aide de jquery. Avant et après l'instruction qui va faire l'appel, nous affichons un message sur la console et nous verrons que le résultat de ces instructions affichent quelque chose comme ça :

```
"We will call the github api for the jquery repo..."
"The call is asynchronous..."
"The first issue on jquery is from: rkatic"
"and its state is: closed"
```

La fonction `callGithubRepo` prend un callback en paramètre et implémente l'appel avec jquery. On aurait [très bien pu l'écrire sans jquery](http://jsbin.com/tikurokuqe/2/edit?html,js,console) :

```
function callGithubRepo(callback) {
  var httpRequest = new XMLHttpRequest();

  httpRequest.onreadystatechange = function() {
    if (httpRequest.readyState === XMLHttpRequest.DONE) {
      if (httpRequest.status === 200) {
        callback(JSON.parse(httpRequest.responseText));
      }
    }
  };

  httpRequest.open('GET', 'https://api.github.com/repos/jquery/jquery/issues/1');
  httpRequest.send();
}
```

## et cætera

### le prochain javascript

Vous le savez peut-être, javascript évolue. Sa syntaxe évolue, ainsi que quelques enrichissements de fonctions et api.

Si vous voulez en savoir plus, [babeljs](https://babeljs.io/docs/learn-es2015/) fait la liste de toutes les nouveautés que vous pouvez d'ores et déjà adopter grâce à babeljs lui-même.

Pour revenir sur les points que nous avons vu, commencez donc par étudier les promesses, qui viennent répondre au même besoin que les `callbacks` en paramètres de fonction pour gérer les appels asynchrones de manière plus explicite, avec par exemple :

```
callGithubRepo().then(onSuccess);
```

Vous pouvez également découvrir les `arrow functions` qui viennent supplanter les fonctions anonymes avec une gestion de `this` plus facile à deviner.

### lire

De nombreux ouvrages pour découvrir ou approfondir son usage de javascript sont disponibles en lignes. Je pense notamment à celui d'[Axel Rauschmayer](https://twitter.com/rauschma), [_speaking javascript_](http://speakingjs.com/), ou celui d'[Addy Osmani](https://twitter.com/addyosmani), [_JavaScript Design Patterns_](http://addyosmani.com/resources/essentialjsdesignpatterns/book/) ou encore [_Eloquent JavaScript_](http://eloquentjavascript.net/) de [Marijn Haverbeke](https://twitter.com/marijnjh).

Ils sont tous référencés sur la [page opensource](https://github.com/revolunet/JSbooks) [JSbooks](http://jsbooks.revolunet.com/).

### awesome javascript

Toujours sur github et parce que la communauté javascript est immense, n'hésitez pas à fouiller dans [awesome-javascript](https://github.com/sorrycc/awesome-javascript), une collection des bibliothèques, ressources, bonnes pratiques autour de javascript.

### outils

J'ai utilisé [jsbin.com](http://jsbin.com) pour proposer des exemples de code qui tourne pour de vrai. Il y en a des tas comme [jsfiddle](https://jsfiddle.net/), [codepen](http://codepen.io/). Ils permettent même de charger des librairies extérieures comme les frameworks à la mode ou jquery.

Apprenez à utiliser les devtools de votre navigateur, que ce soit celui de [Google](https://developers.google.com/web/tools/chrome-devtools/) ou celui de [Mozilla](https://developer.mozilla.org/en-US/docs/Tools) ou celui de [Microsoft](https://dev.windows.com/en-us/microsoft-edge/platform/documentation/f12-devtools-guide/).

### One more thing: use strict

Vous voilà armé pour (re)commencer votre découverte de javascript. Si je peux vous recommander une dernière chose, utilisez toujours le mode strict dans des fonctions IIFE pour clore la portée de vos déclarations :

```
(function() {
  'use strict';

  // un monde plus sûr

})();
```

# Annexes
image d'illustration : http://developers.do/wp-content/uploads/2012/01/wat.png
