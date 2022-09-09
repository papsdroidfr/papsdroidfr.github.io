---
#source: https://www.papsdroid.fr/post/tutoriel-piothello
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      comments: true
      share: true
      related: true
header: 
  teaser : "/assets/images/tutos/011PiOthello/piothello-300.jpg"

#SEO tags
title       : "Pi Othello"
image       : "/assets/images/tutos/011PiOthello/piothello-300.jpg"
description : "Affrontez un Raspberry pi à l'Othello"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Pi Othello"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "IA" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pi" , "python3", "électronique", "matrice leds", "HAT" ]

gallery1:
  - image_path: "/assets/images/tutos/011PiOthello/Kicad_GERBER_Top.png"
  - image_path: "/assets/images/tutos/011PiOthello/Kicad_GERBER_bottom.png"
---
![piOthello](/assets/images/tutos/011PiOthello/piothello-300.jpg){: .align-left} 
Je vais détailler dans ce post comment fabriquer un petit jeux d'Othello avec un Raspberry pi
<!--more-->
, une matrice de 8*8 leds bicolor d'Adafruit et une intelligence artificielle à plusieurs niveaux (facile/moyen/difficile) qui permet à un joueur humain de se mesurer à la machine, ou de voir à l'oeuvre deux IA s'affronter.
{: .text-justify}

## Matériel nécessaire
- 1 Raspberry pi modèle 3b+ (ça devrait aussi fonctionner sur un pi3b et sur un pi4), avec Raspbian installé sur un SD card 16Go, ainsi que son alimentation.
{: .text-justify}
- Le circuit imprimé format HAT piOthello, à récupérer sur ma [page Guithub](https://github.com/papsdroidfr/PiOthello) (fichier GERBER piOthelloGERBER.zip), à faire fabriquer chez un pro (comptez une vingtaine d'euros frais de port compris). Vous pouvez aussi me contacter pour en récupérer un tout prêt, vu que je les fais imprimer en 5 exemplaires à chaque fois.
{: .text-justify}
- 3 boutons poussoirs 12mm, 1 noir (Off), 1 jaune (Menu), 1 vert (Validation)
{: .text-justify}
- Résistances 1/4w: 6*10kΩ, 2*220Ω
{: .text-justify}
- Leds 5mm: 1 rouge et 1 verte (pas besoin de support de leds)
{: .text-justify}
- Condensateur : 1*100nf céramique (condensateur de découplage pour la matrice de leds)
{: .text-justify}
- 1 barrette 2*20 pin 2,54mm femelle (pour la connexion au GPIO du Rapsberry pi)
{: .text-justify}
- Matrice de leds 8*8 bicolor Adafruit avec un backpack i2C: [ce modèle](https://www.adafruit.com/product/902). Vous la trouverez pour environ 20€ dans les boutiques spécialisées de revente de matériel Arduino/Raspberry.
{: .text-justify}
- 2 petites entretoises M2.5 10mm femelle-femelle pour assurer le support de la carte HAT (2 suffise puisque l'autre côté sera maintenu par la barrette 2*20pin). Attention sans entretoises vous prenez le risque de tordre la carte lors des appuis sur les boutons poussoirs avec un gros risque que les soudures du dessous aillent toucher un truc métallique du Raspberry pi, ce qui provoquerait un court-circuit préjudiciable.
{: .text-justify}
- 4 petites entretoises M2.5 5mm mâle/femelle qui serviront de petits pieds sous le Raspberry pi, 2 d'entre elles seront reliées aux entretoises 10mm du dessus, et 2 autres il faut utiliser des petits boulons M2.5 pour les fixer.
{: .text-justify}

## Circuit électronique

![piOthello](/assets/images/tutos/011PiOthello/Kicad_schema.png){: .align-center} 
{% include gallery id="gallery1" caption="PCB" %}

Il y a 3 boutons poussoir reliés aux pins GPIO26 (Off), GPIO17(CHX = choix menu) et GPIO12 (VALID = validation) avec 2 résistances de 10kΩ pour chacun. Ces boutons transmettent l'information GPIO.LOW lorsqu'ils sont activés, et le programme python gère l'anti-rebond de manière logicielle, d'où la raison pour laquelle des condensateurs anti-rebond sont inutiles ici.
{: .text-justify}

Les 2 leds sont reliées quand à elles aux GPIO20(led rouge) et GPIO21(led verte) avec leurs résistances de protection 220Ω en série.
{: .text-justify}

La matrice de 8*8 leds bicolor d'adafruit est connectée aux 4 ports GND, +3.3V et SDL, SLA (port I2C) du Raspberry pi, avec un condensateur de découplage entre la masse et l'alim du circuit intégré qui gère cette matrice. Son format mini est parfaitement adapté pour un HAT.
{: .text-justify}

Enfin par dessous la carte il faut souder la barrette 2*20 pin 2,54mm femelle qui sera reliée au prot GPIO du Rapsberry
{: .text-justify}

Le circuit intégré a été conçu en double couche sous Kicad au format HAT pour Raspberry, avec plan de masse recto GND et plan +3.3V au verso. LEs fichiers GERBER fournis sur ma page Github permettent de le faire réaliser chez n'importe quel professionnel 
{: .text-justify}

{% include figure image_path="/assets/images/tutos/011PiOthello/breadboard.jpg" caption="Ce circuit est facilement réalisable sur une breadboard" %}

Pour souder les composants de la matrice de leds (matrice + support backpack + pin header 4pin) il est important de suivre le tutoriel d'Adafruit à la lettre, il est très bien expliqué pour éviter de souder les composants dans le mauvais sens et pour faire des soudures parfaitement droites en utilisant une breadboard: très pratique !
{: .text-justify}

Ensuite pour souder les composants sur le circuit imprimé tout est clairement sérigraphié dessus / dessous: impossible de se tromper.
{: .text-justify}

**Astuce**: pour souder le pin header 4 pin du backpack de la matrice de leds sur le circuit imprimé, je vous conseille d'utiliser des cure-dents à disposer dans les 4 petits trous dans les coins, afin de maintenir la matrice parfaitement orientée sans qu'elle bouge pendant les 4 petites soudures à faire. Sinon vous prenez le risque de souder de travers la matrice.
{: .text-justify}

## Installation du système et programmes Pythons

Il faut d'abord installer les bibliothèques Adafruit qui vont permettre de gérer la matrice: suivez bien [leur guide](https://learn.adafruit.com/matrix-7-segment-led-backpack-with-the-raspberry-pi/overview). Il n'y a rien de compliqué. Pensez à bien activer les ports I2C du Raspberry pi comme epxliqué, et il y a même un petit programme de démo que vous pouvez tester.
{: .text-justify}

Les programmes pythons sont à récupérer sur ma [page Github](https://github.com/papsdroidfr/PiOthello).
{: .text-justify}

3 programmes sont nécessaires:
- **piOthello.py**: c'est le programme principal à exécuter
- **ledMatrixBicolor.py**: classe qui gère la matrice de leds
- **game.py**: gestion du jeux Othello

Pour exécuter ce programme il suffit d’exécuter une commande: **python3 piOthello.py**, ou bien de rajouter dans le fichier **/etc/rc.local** cette ligne: **python3 /home/pi/piOthello.py &** (sans oublier le & à la fin ...) juste avant le "exit 0", en supposant que les 3 programmes pythons sont déposés dans le répertoire/home/pi/ ainsi le programme s'éxécute à chaque démarrage du Raspberry.
{: .text-justify}

## Explications sur le fonctionnement du jeux

Après une brève petite animation, le système demande quels sont les joueurs P1/Rouge et P2/Vert qui vont jouer: utiliser le bouton Jaune (MENU) pour fiare un choix parmi la liste proposée via des logos qui s'affichent:
{: .text-justify}

![piOthello](/assets/images/tutos/011PiOthello/logo1.jpg){: .align-left}
Logo choix = Humain: ce joueur va devoir jouer en utilisant les boutons poussoirs:
{: .text-justify}
- bouton Jaune (MENU): choisir un coup parmi une liste de coups possibles qui clignotent en orange. chaque appui montre un nouveau coup possible parmi la liste des coups possibles du joueur.
{: .text-justify}
- bouton Vert (VALID): validation du coup à joueur.
{: .text-justify}

![piOthello](/assets/images/tutos/011PiOthello/logo2.jpg){: .align-left}
Ce logo représente le premier niveau d'IA qui est assez bête puisqu'elle va jouer un coup au hasard parmi ses coups possibles. Pratique pour débuter.
{: .text-justify}


![piOthello](/assets/images/tutos/011PiOthello/logo3.jpg){: .align-left}
Celui-ci plus coriace avec ses yeux rouges va jouer le meilleur coup stratégique, sans toutefois anticiper les prochains coups de son adversaire. Pour cela il va en début de jeux pondérer la matrice pour focaliser sur les pièces stratégiques à prendre, telles que les coins ou 2 cas avant les coins ..., puis en fin de partie le bougre va changer de stratégie pour choper le plus de pions possible puisque c'est le but.
{: .text-justify}


![piOthello](/assets/images/tutos/011PiOthello/logo4.jpg){: .align-left}
Attention celui-là il est furieusement excité à jouer le meilleur coup stratégique en anticipant les 5 prochains coups de son adversaire: il faut se cramer la tête pour le battre !
{: .text-justify}

Il s'agit d'un algorithme de [recherche récursive de type Min-Max avec élagage alpha-bêta](https://fr.wikipedia.org/wiki/Algorithme_minimax). 
{: .text-justify}

Concernant le programme d'IA de recherche min-max avec élagage alpha-bêta je ne suis pas partie d'une feuille blanche: on trouve sur le net des tonnes de programmes déjà écrits et j'ai sélectionné le meilleur à mon goût, mis à disposition [ici](https://github.com/dhconnelly) en libre service par un ingénieur Google avec une remarquable modélisation du tableau de jeu Othello, rien d'autre n'arrive à la cheville de ce qu'il a fait. J'ai juste du adapter son programme écrit en python2 à l'époque pour le transformer en python3, puis le transformer en orienté objet, et adopter les stratégies afin que l'IA cherche les meilleures places stratégiques en début de partie, puis bascule sur une stratégie de recherche du maximum de pièces en fin de partie: c'est assez redoutable.
{: .text-justify}

Enfin lors du jeux, la led allumée correspond au joueur en train de jouer.
{: .text-justify}

Les coups possibles clignotent quelques instants en orange avant de faire son choix
{: .text-justify}

Le coup en cours de validation clignote dans la couleur du joueur.
{: .text-justify}

## Affrontement 2 IA en vidéo
{% include video id="KQE6VzJ6h1w" provider="youtube" %}