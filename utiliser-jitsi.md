---
title: Utiliser Jitsi
url: /blog/metier/2020/utiliser-jitsi
---

*Dans le contexte actuel, les individus ont de plus en plus l'occasion de communiquer grâce à
[Jitsi Meet][]. Voici quelques astuces qui peuvent rendre son utilisation
plus confortable et pour tout le monde.*

## Éviter Firefox

Une fois encore, il est dommage que Jitsi ne valorise pas cet **excellent navigateur**.
En effet, un détail du protocole utilisé par Jitsi n'a pas encore été implémenté et celui-ci 
ne sera pas inclus dans [Firefox][] avant le mois de juin. En attendant, Jitsi
reste utilisable avec [Firefox][], mais son utilisation a un **impact négatif** sur la
qualité audio et vidéo de tous les participants à la conférence.

+ INFOS : l'article [Vidéoconférences : Firefox et Jitsi
travaillent à un meilleur support][nextinpact].

En attendant donc que [Firefox][] et Jitsi règlent ce désagrément, privilégier
Chromium, ou bien la solution suivante :

[nextinpact]: https://www.nextinpact.com/brief/videoconferences---firefox-et-jitsi-travaillent-a-un-meilleur-support-11906.htm

----

## Jitsi Meet Electron

[Electron][] est une *coquille* pour transformer une *application web*
en application multi-plateformes autonome.

[Jitsi Meet Electron](https://github.com/jitsi/jitsi-meet-electron) est donc une
version de [Jitsi Meet][] enrobée par [Electron][], qui permet d'utiliser cet
outil de visioconférence sans avoir à passer par un navigateur web[^1].

Cet outil utilise par défaut le serveur principal de Jitsi, mais autorise
également à configurer n'importe quel serveur Jitsi[^2].

![Aperçu de l'interface de Jitsi Meet Electron](https://raw.githubusercontent.com/jitsi/jitsi-meet-electron/master/screenshot.png)

### Note

Jitsi propose également [sa propre version Desktop][jitsi-desktop], mais
elle nécessite l'installation de nombreuses dépendances supplémentaires. Je
n'ai pas eu l'occasion pour le moment de la tester.

----

## Astuces

### La vidéo

Si les utilisateurs n'ont pas besoin forcement de se voir les uns les autres, il est utile de
**désactiver sa propre caméra**. Cela permet une grosse économie de bande
passante et ainsi de garantir une meilleure qualité audio.

Dans les paramètres, **qualité de la vidéo**[^3] peut être ajuster. Ce réglage
influence uniquement les flux vidéos que l'internaute reçoit et non sur les flux
envoyés.

![Barre d'outils de Jitsi mettant en avant le bouton contrôlant la vidéo](https://res.cloudinary.com/ma-b/image/upload/v1588171292/blog-posts/video_xh7dld.png)

### L'audio

De même, si on est nombreux, il n'est pas utile de garder son micro activé en
permanence. On peut donc prendre l'habitude de **couper le micro**.

![Barre d'outils de Jitsi mettant en avant le bouton contrôlant le son](https://res.cloudinary.com/ma-b/image/upload/v1588171292/blog-posts/audio_hppmcs.png)

----

## Au clavier

L'activation et la désactivation de l'audio / vidéo peuvent se faire via les
boutons du milieu, au bas de l'interface. Cette bascule peut également se
faire rapidement à l'aide des touches du clavier : **M** pour le micro, et **V**
pour la vidéo.

Il est également possible de rester appuyé sur la **touche espace** et ainsi
être en mode **push-to-talk** *(Que le micro soit initialement activé ou non,
la touche espace coupe le micro lorsqu'elle est relâchée)*.

La plupart des raccourcis clavier peuvent être retrouvés grâce à la touche **?**.

![Capture d'écran de la liste des raccourcis clavier](https://res.cloudinary.com/ma-b/image/upload/v1587574895/blog-posts/clavier_hw7vpp.png)

----

## Lever la main

À cause de la latence entre émission et réception, l'intéraction à plusieurs personnes est parfois compliquée. Les utilisateurs se coupent la parole ou commencent à parler
en même temps.

Dans d'autres cas comme au cours d'une présentation par l'un des participants, les autres utilisateurs ne savent pas s'il est possible de l'interrompre (ou non), il en est de même pour poser une
question.

Pour celà Jitsi propose une fonctionnalité permettant de **lever la main**. La ou les
personnes menant les échanges et animant la discussion peuvent alors donner la
parole, au moment opportun, à celles qui se sont manifestées ainsi.

![Barre d'outils de Jitsi mettant en avant le bouton permettant de lever la main](https://res.cloudinary.com/ma-b/image/upload/v1588171292/blog-posts/leverlamain_hmfieu.png)

*Penser à baisser la main une fois qu'on s'est exprimé.*

Cette fonctionnalité est accessible via un bouton en bas à gauche de
l'interface ou bien via la touche **R**.

----

## Mosaïque

Dès lors qu'il y a plus de 3 participants et qu'il n'y a rien à *voir* (pas de
diffusion vidéo), le **mode mosaïque** - accessible via un bouton en bas à
droite de l'interface - permet de se rendre compte plus facilement de qui
s'exprime à quel moment.

![Barre d'outils de Jitsi mettant en avant le bouton contrôlant de mode d'affichage](https://res.cloudinary.com/ma-b/image/upload/v1588171292/blog-posts/mosaique_qm4ta1.png)

[^1]: En interne, [Electron][] utilise un pseudo-Chromium.
      Pour plus de détail, voir le site d'[Electron][].

[^2]: À condition que soit activée la fonctionnalité *External API* sur le
      serveur.

[^3]: Ce réglage n'est disponible qu'en dehors du mode peer-to-peer qui s'active
      automatiquement lorsqu'il n'y a que deux participants.

[Electron]: https://www.electronjs.org/
[Firefox]: https://www.mozilla.org/fr/firefox/features/
[Jitsi Meet]: https://meet.jit.si/
[jitsi-desktop]: https://desktop.jitsi.org/Main/Download

----

Une erreur, une question, une remarque, n'hésitez pas :
[@mab_](https://twitter.com/mab_) ou directement par une
[pull-request](https://github.com/makinacorpus/blog-posts/blob/master/utiliser-jitsi.md).
