---
url: /blog/societe/2016/geotrek-bilan-dune-annee-pleine-de-nouveautes
---

# Geotrek v2, les nouveautés

Les années 2015 et 2016 a été riches en améliorations pour Geotrek. C'est une véritable satisfaction de voir un projet open-source avancer autant !
En essayant de ne pas trop en faire, voilà une petite liste de ce que la communauté et Makina ont accomplis ensemble.

## Geotrek à plusieurs

Cette année, la mutualisation est à l'honneur avec des instances regroupant plusieurs structures pour une gestion et une contribution unifiée. Il y a notamment :

* [Chemins de parcs](http://presse.provenceguide.com/wp-content/uploads/2016/06/DP-Chemins-des-Parcs-juin-2016.pdf) regroupant 5 PNR de PACA, et bientôt 7.
* [Rando Grands Causses](http://rando.parc-grands-causses.fr/) qui travaille déjà avec les offices de tourisme du territoire pour la saisie des contenus
* L'APEM qui travaille sur un [déploiement de Geotrek sur l'ensemble du massif pyrénéen](http://www.apem.asso.fr/actus-page-d-accueil/82-25-novembre-2015-%C3%A9change-intermassif-sur-l%E2%80%99utilisation-de-geotrek).
* Maison du tourisme du Champsaur Valgaudemar qui saisit dans le Geotrek-admin du Parc national des Ecrins mais proposera bientôt son propre portail Geotrek-rando
* Le [Parc national de la Vanoise](http://rando.vanoise.com/) qui travaille à la mise en place de partenariats avec d'autres structures des territoires alentours


## Refonte du site internet
Grande nouvelle ! Le site http://geotrek.fr/ a fait peau neuve cet été !

Outre le rafraichissement graphique nécessaire, il nous a paru indispensable de décoreller Geotrek de Makina Corpus. Bien qu'étant le principal prestataire et contributeur, il nous semble important d'affirmer qu'avant tout Geotrek est un produit open-source créé, maintenu, et porté par sa communauté.

Nous prévoyons la création d'une page sur notre site, dédiée à nos prestations Geotrek. En attendant, si vous avez la moindre question, vous pouvez nous contacter à l'adresse [projets-sig@makina-corpus.com](mailto:projets-sig@makina-corpus.com)

## Geotrek et l'IGN

Il est désormais possible d'exporter le contenu de Geotrek vers l'espace loisir IGN. C'est une avancée plus qu'agréable pour les utilisateurs. Plus besoin de dupliquer la donnée !

Merci aux structures qui l'utilise déjà et nous ont aidé à le rendre possible :
* [Le Parc national des Écrins](http://www.grand-tour-ecrins.fr/)
* [Parc National du Mercantour](http://rando.mercantour.eu/)
* [Conseil Général de la Loire Atlantique](http://rando.loire-atlantique.fr/)
* [Comité Régional du Tourisme de Champagne-Ardenne](http://rando-champagne-ardenne.com/)
* [Parc Naturel Régional des Grands Causses](rando-champagne-ardenne.com/)

## Les nouvelles fonctionnalités

Qui dit nouveautés dit nouvelles fonctionnaliés ! Il serait trop long de toutes les recenser ici, néanmoins voilà les principales à retenir.

### Admin
* Possibilité de localiser des infos pratiques sur les tronçons pour les afficher sur le portail Geotrek-rando (points d'eau, passages délicats...)
* Possibilité de publier sur différents portails Geotrek-rando depuis un même Geotrek-admin
* [Éditeur WYSIWYG](https://geotrek.readthedocs.io/en/master/user-manual.html) pour les pages statiques basé sur bootstrap pour la gestion du responsive.
* Possibilité de créer des séjours itinérants auxquels sont rattachés les différentes étapes ([Exemple du parc des Écrins](http://www.grand-tour-ecrins.fr/a-pied/itinerance-alpine-en-valgaudemar/))
* Possibilité de renseigner un système et un identifiant de réservation pour les contenus touristiques
* Un nouveau système de génération des PDF utilisant [WeasyPrint](http://weasyprint.org/). Cela permet de prendre dirèctement les pages HTML/CSS au lieu de documents ODT. ([Exemple du parc des Cévennes](http://destination.cevennes-parcnational.fr/data/api/fr/treks/38033/chemin-de-memoires.pdf))
* Intégration d'un [module d'import](https://geotrek.readthedocs.io/en/master/import.html)
* Amélioration des imports de contenus touristiques depuis Apidae (ex-Sitra) pour intégrer plus d'informations ([Exemple du parc des Écrins](http://www.grand-tour-ecrins.fr/contenu-touristique/chabourneou/))
* Possibilité de [lisser les calculs des altitudes](https://github.com/makinacorpus/Geotrek/issues/1452)
* Possibilité de publier les lieux de renseignements sur les cartes de Geotrek-rando ([Exemple du parc du Mercantour](http://rando.mercantour.eu/?categories=T4))
* Possibilité de limiter les droits de publication de certains utilisateurs pour les soumettre à la validation avant publication
* Affichage des longueurs 2D et 3D des linéaires
* L'ajout d'un champ ETAT sur les aménagements et la signalétique
* Possiblité de fusionner 2 tronçons

Et pour le reste : https://github.com/makinacorpus/Geotrek/releases

### Rando
* L'affichage de contenus et évènements touristiques importés ou saisis directement dans Geotrek-Admin ([Exemple du parc des Cévennes](http://destination.cevennes-parcnational.fr/))
* L'ajout des fonctions liées à l'itinérance ([Exemple du parc des Écrins](http://www.grand-tour-ecrins.fr/))
* L'affichage des infos pratiques (Exemple en bleu sur la carte, d de http://itinerance.alpesrando.net/pedestre/balcons-de-serre-poncon-a-pied/)
* L'affichage des widgets de réservation en ligne FFCAM et OpenSystem ([Exemple sur Itinérance Alpes Rando, à la fin de la fiche](http://itinerance.alpesrando.net/contenu-touristique/camping-le-petit-liou/))

Plus de détails sur https://github.com/makinacorpus/Geotrek-rando/releases

### Mobile
* Possibilité d'intégrer les contenus et événements touristiques dans l'application mobile
* Intégration de l'itinérance (fiches séjours et fiches étapes)
* Stabilité accrue et améliorations de l'ergonomie

Plus d'infos sur https://github.com/makinacorpus/Geotrek-mobile/releases

## Le futur
En 2017, plusieurs structures souhaitent travailler sur [l'itinérance à la carte](https://github.com/makinacorpus/Geotrek-rando/issues/346), la commercialisation d'hébergements, l'offre verticale (alpinisme, escalade...) ou encore l'amélioration des possibilités d'import.

Une idée ou un problème ? Parlez en grâce au dépots githubs [Admin](https://github.com/makinacorpus/Geotrek/issues), [Rando](https://github.com/makinacorpus/Geotrek-rando/issues), et [Mobile](https://github.com/makinacorpus/Geotrek-mobile/issues).

Un besoin de développement ou d'aide ? Contactez nous pour échanger et prévoir un projet ensemble !

## Conclusion

Ces deux dernières années ont été très fructueuses en terme d'évolution et ont vu arriver la nouvelle génération de produits Georek. Cette suite contient beaucoup de belles prouesses qui intéressent et stimulent nos équipes. Surtout un grand merci à la communauté sans qui tout cela ne serait possible. Makina Corpus est fier d'être l'un des pilier de ce beau projet !
