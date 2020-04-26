---
url: /blog/metier/2016/repatelier-gpg
---

Nous avons accueilli Ludovic Hirlimann, développeur chez Mozilla dans l'équipe Thunderbird, pour nous parler de OpenGPG. Autour d'un repas, il nous a présenté les intérêts des signatures sécurisées PGP, l'utilisation de l'outil GPG pour gérer son trousseau de clés et fait une petite démonstration de signature croisée de clés.

# Pourquoi signer ?

Nous n'avons pas besoin d'avoir de grands pouvoirs pour avoir de grande responsabilité. Et quand Ludovic nous explique que si tout le monde signait ses emails, il y aurait moins de spam, _phishing_ (hameçonage) compris, l'intérêt est explicite. ![](https://upload.wikimedia.org/wikipedia/en/7/7a/MontySpam.jpg)

Mais sans forcément virer dans l'utopie, l'intérêt d'une signature PGP est la même que celle d'un certificat SSL : avoir confiance en l'auteur du contenu. Et pour un éditeur de logiciel, pour des développeurs, ça peut être très utile de propager confiance et assurance.

Avec une paire de clés PGP, il est également possible de proposer à n'importe qui de nous envoyer un message chiffré que nous serons les seuls à pouvoir lire. Les tentatives de contrôle d'internet des pays même les plus _républicains_ sont une démonstration de l'utilité de ce genre de prévention. Les lanceurs d'alerte ne savaient pas qu'ils allaient l'être avant de se retrouver dans de sales draps. Nos correspondants peuvent avoir n'importe quelle raison pour vous contacter avec un message chiffré et si nous leur laissons la possibilité de le faire, c'est déjà ça.

# PGP, OpenPGP, GPG ? C'est un peu compliqué

**PGP** veut dire _Pretty Good Privacy_, c'est le premier programme qui a permis de définir le standard **[OpenPGP](https://fr.wikipedia.org/wiki/OpenPGP)**. [PGP](https://fr.wikipedia.org/wiki/Pretty_Good_Privacy) comme son standard a été proposé par [Phil Zimmermann](https://fr.wikipedia.org/wiki/Philip_Zimmermann) entre 1991 et 1997. PGP appartient maintenant à [Symantec](https://www.symantec.com/products/information-protection/encryption).

La fondation de [Richard Stallman](https://fr.wikipedia.org/wiki/Richard_Stallman) [Free Software Foundation](https://fr.wikipedia.org/wiki/Free_Software_Foundation) a proposé sa propre implémentation du standard OpenPGP appelée _GNU PRivacy Guard_ ou **GPG**. C'est donc cette implémentation qu'il est recommandé d'utiliser puisque son développement est actif et que son code est ouvert et libre.

# Comment ça marche ?

Le chiffrement asymétrique est au centre d'OpenPGP. L'idée est la suivante :

1. nous générons deux clés, l'une privée et personnelle, l'autre publique ;
2. nous publions la clé publique à qui veut ;
3. nous gardons précieusement la clé privée.

Lorsque quelqu'un voudra nous envoyer un message que nous serons les seuls à pouvoir lire, il en chiffrera le contenu avec notre clé publique. Seule notre clé privée permettra de déchiffrer ce message.

Autre cas d'utilisation : si nous voulons certifier que nous sommes l'expéditeur d'un message, nous pouvons créer une empreinte du contenu, et la chiffrer avec notre clé privée, c'est ce qu'on appellera la signature à proprement parler. Le destinataire pourra également de son coté générer un empreinte et la comparer à celle qu'il obtient en déchiffrant la signature. Si il y a correspondance entre les deux empreintes, le destinataire pourra être sûr de deux choses grâce à cette signature numérique : c'est notre clé privée qui a permis de générer l'empreinte chiffrée du message __et__ le contenu du message est le même que celui que nous avons envoyé (personne ne l'a altéré).

# Et en pratique

En pratique nous nous reposerons sur un système Open Source comme Linux sur lequel nous installerons le programme GPG. Les étapes citées ci-dessous proviennent de la documentation recommandée par Ludovic : http://keyring.debian.org/creating-key.html. Elle-même étant basée sur [le billet de blog de Ana Beatriz Guerrero López](http://ekaia.org/blog/2009/05/10/creating-new-gpgkey/).

## Créer une nouvelle clé GPG.

_Il vous incombe, selon votre distribution, d'[installer GPG](https://gnupg.org/download/index.html) avant de continuer._

Bien qu'il puisse y avoir des raisons d'utiliser une clé sur 2048 bits, il est recommandé d'utiliser une clé sur 4096 bits, non pas uniquement parce que la clé est plus longue, mais aussi parce que l'algorithme est plus solide.

Nous allons commencer par mettre à jour GPG pour utiliser SHA2. Pour ce faire, ajoutons à la fin du fichier `~/.gnupg/gpg.conf` :

```
personal-digest-preferences SHA256
cert-digest-algo SHA256
default-preference-list SHA512 SHA384 SHA256 SHA224 AES256 AES192 AES CAST5 ZLIB BZIP2 ZIP Uncompressed
```

Lançons la commande de génération de clés :

```
gpg --gen-key
```

Depuis GnuPG 1.4.0+, l'option par défaut est "RSA", quelle que soit la version que vous utilisez, il faut choisir la génération d'une clé RSA.

Renseignons les informations que GPG nous demande (Nom réel, adresse mail et commentaire) et validons ces informations. GPG nous demande alors de choisir une _passphrase_, c'est à dire un mot de passe permettant de protéger notre clé privée.

À la fin de la génération de notre paire de clé, GPG vous précisera qu'il serait bon d'éditer notre nouvelle clé pour le chiffrement ou pour ajouter un UID.

Voici l'exemple d'Ana Beatriz Guerrero López qui va nous permettre de prendre des repères pour la suite :

```
pub   4096R/6AA15948 2009-05-10
      Key fingerprint = 7A33 ECAA 188B 96F2 7C91  7288 B346 4F89 6AA1 5948
uid                  Ana Beatriz Guerrero López <ana@ekaia.org>
```

## Ajouter un autre UID

Pour ajouter une autre adresse mail (une autre identité) à notre jeu de clé :

```
gpg --edit-key 0x6AA15948
 …
command> adduid
```

_Remplacez 0x6AA15948 par l'empreinte de votre clé._

GPG nous demande toutes les informations nécessaires à notre nouvel UID : nom, mail et commentaire. GPG nous demandera notre passphrase pour dévérouiller notre clé. Enfin nous devrons sauvegarder notre clé.

```
command> save
```

## Ajouter une sous-clé pour le chiffrement

Nous devons éditer notre clé derechef :

```
gpg --edit-key 0x6AA15948
 …
Command> addkey
```

Après avoir dévérouillé la clé avec la _passphrase_, nous ajouterons une nouvelle clé pour le chiffrement (_encryption_), choix 6. Nous choisirons une longueur de 4096 bits pour cette nouvelle clé. Enfin nous choisirons la date d'expiration (jamais). Nous validerons tout nos choix et nous sauvegarderons la clé avec :

```
command> save
```

## Publier la nouvelle clé sur le serveur

Le serveur est configuré dans le fichier `~/.gnupg/gpg.conf`, par défaut `keyserver hkp://keys.gnupg.net`. Pour envoyer la clé, nous exécuterons la commande GPG suivante :

```
gpg --send-key 6AA15948
```

_Là encore, vous adapterez l'empreinte de votre propre clé._

# Bibliothèques de liens

- Creating a new GPG key http://keyring.debian.org/creating-key.html
- OpenPGP keyserver http://keys.gnupg.net/
- SSH: Best practices https://blog.0xbadc0de.be/archives/300
- PGP pathfinder & key statistics http://pgp.cs.uu.nl/
- Biglumber - key signing coordination http://biglumber.com/
- The GNU Privacy Handbook https://www.gnupg.org/gph/en/manual/book1.html

# Le repatelier

C'était un véritable plaisir d'accueillir Ludovic Hirlimann dans nos locaux. Nous pourrons désormais divulguer la bonne parole et organiser des [_booms de signatures_](https://fr.wikipedia.org/wiki/Key_signing_party) (_key signing party_).

Un grand merci à lui.
