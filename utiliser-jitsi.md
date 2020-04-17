---
title: Utiliser Jitsi
description:  On a de plus en plus l'occasion de communiquer grâce à Jitsi Meet. Voici quelques astuces qui peuvent rendre son utilisation un peu plus confortable pour tous.
url: /blog/metier/2020/utiliser-jitsi
---

*Dans le contexte actuel, on a de plus en plus l'occasion de communiquer grâce à
[Jitsi Meet][]. Voici quelques astuces qui peuvent rendre son utilisation un peu
plus confortable pour tous.*

## Eviter Firefox

C'est dommage de, pour une fois, ne pas valoriser cet **excellent navigateur**,
mais un détail du protocol utilisé par Jitsi n'a pas encore été implémenté, et
ne sera pas inclu dans [Firefox][] avant le mois de juin. En attendant, Jitsi
reste utilisable avec [Firefox][], mais cela a un **impact négatif** sur la
qualité audio et vidéo de tous les participants à la conférence.

Pour plus d'informations, voir l'article [Vidéoconférences : Firefox et Jitsi
travaillent à un meilleur support][nextinpact]

En attendant donc que [Firefox][] et Jitsi règlent ce désagrément, privilégier
Chromium, ou bien la solution suivante...

[nextinpact]: https://www.nextinpact.com/brief/videoconferences---firefox-et-jitsi-travaillent-a-un-meilleur-support-11906.htm

## Jitsi Meet Electron

[Electron][] est une *coquille* qui permet de transformer une *application web*
en application multi-plateforme autonome.

[Jitsi Meet Eletron](https://github.com/jitsi/jitsi-meet-electron) est donc une
version de [Jitsi Meet][] enrobée par [Electron][], qui permet d'utiliser cet
outil de visioconférence sans avoir à passer par un navigateur web[^1].

Cet outil utilise par défaut le serveur principal de Jitsi, mais autorise
également à configurer n'importe quel serveur Jitsi[^2].

### Note

Jitsi propose également [sa propre version Desktop][jitsi-desktop], mais
celle-ci necessite l'installation de nombreuses dépendances suplémentaires. Je
n'ai pas l'occasion pour le moment de la tester.

## Astuces

*(Fonctionnent aussi bien avec la version web et la version Eletron de Jitsi.)*

### La vidéo

Si on a pas besoin forcement de se voir les uns les autres, il est utile de
**désactiver sa propre caméra**. Cela permet une grosse économie de bande
passante, et ainsi de garantir une meilleure qualité audio.

Dans la paramètres, on peut ajuster la **qualité de la vidéo**[^3]. Ce réglage
n'a d'influence que sur les flux vidéos qu'on reçoit, et non sur celui qu'on
envoie.

### L'audio

De même, si on est nombreux, il n'est pas utile de garder son micro activé en
permanence. On peut donc prendre l'habitude de **couper le micro**.

### Au clavier

L'activation et la désactivation de l'audio / vidéo peuvent se faire via les
boutons du milieu, au bas de l'interface. Cette bascule peut également se faire
à l'aide des touches du clavier : **M** pour le micro, et **V** pour la vidéo.

#### Push-to-talk

Il est également possible de rester appuyé sur la **touche espace** et ainsi
être en mode **push-to-talk**. *(Que le micro soit initialement activée ou non,
la touche espace coupe le micro lorsqu'on la relache)*

### Lever la main

À cause de la latence entre émission et réception, il est parfois compliqué
d'interagir à plusieurs personnes. On se coupe la parole ou on commence à parler
en même temps, plusieurs fois de suite.

Dans d'autres cas, par exemple au cours d'une présentation par l'un des
participant et on ne sait pas si on peut l'interompre ou non pour poser une
question.

Jitsi propose une fonctionnalité permettant de **lever la main**. La ou les
personnes menant les échanges, animant la discussion peuvent alors donner la
parole, au moment opportun, à celles qui se sont manifestées ainsi.

*Penser à baisser la main une fois qu'on s'est exprimé.*

Cette fonctionnalité est accessible via un bouton en bas à gauche de
l'interface, ou bien via la touche **R**.

### Mosaïque

Dès lors qu'il y a plus de 3 participants et qu'il n'y a rien à *voir* (pas de
diffusion vidéo), le **mode mosaïque**, accessible via un bouton en bas à
droite de l'interface, permet de se rendre compte plus facilement de qui
s'exprime à quelle moment.

[^1]: En interne, [Electron][] utilise un pseudo-Chromium.  
      Pour plus de détail, voir le site d'[Electron][]
[^2]: À condition de soit activée la fonctionnalité *External API* sur le
      serveur.
[^3]: Ce réglage n'est disponible qu'en dehors du mode peer-to-peer qui s'active
      automatiquement lorsqu'il n'y a que deux participants.

[Electron]: https://www.electronjs.org/
[Firefox]: https://www.mozilla.org/fr/firefox/features/
[Jitsi Meet]: https://meet.jit.si/
[jitsi-desktop]: https://desktop.jitsi.org/Main/Download

---

Une erreur, une question, une remarque, n'hesitez pas : [@mab_](https://twitter.com/mab_) ou directement par une [pull-request](https://github.com/makinacorpus/blog-posts/blob/master/utiliser-jitsi.md).
