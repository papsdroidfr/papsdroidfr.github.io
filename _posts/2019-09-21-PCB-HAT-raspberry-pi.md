---
#source: https://www.papsdroid.fr/post/tutoriel-pcb-format-hat-pour-raspberry-pi
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
  teaser : "/assets/images/tutos/004HAT/HAT_300.jpg"

#SEO tags
title       : "HAT raspberry pi"
image       : "/assets/images/tutos/004HAT/HAT_300.jpg"
description : "PCB format HAT pour Raspberry pi 3 et pi 4"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "HAT"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pi" ,  "python3", "électronique", "fan", "HAT"]

#galerie schéma
gallery1:
  - image_path: "/assets/images/tutos/004HAT/piOffHAT_schema.png"
  - image_path: "/assets/images/tutos/004HAT/piOffHAT_routage.png"

---

![HAT](/assets/images/tutos/004HAT/HAT_300.jpg){: .align-left} 
Une carte PCB au format HAT c'est quand même bien plus sexy qu'une carte reliée avec une nappe GPIO 40 broches. 
Je présente dans ce tutoriel un modèle conçu sous KICAD, à utiliser pour concevoir de telles cartes qui respectent la norme HAT 
<!--more-->
publiée par la fondation Raspberry pi. En bonus un petit exemple avec le [projet piOff](https://papsdroidfr.github.io/tutoriels/pioff/) déjà publié, que je propose ici dans une **version HAT**, avec en prime une gestion intelligente d'un ventilateur.
{: .text-justify}

## C'est quoi la norme HAT?

Il s'agit des spécifications détaillées qui permettent de concevoir un PCB qui se loge pile poil au dessus du Raspberry sur ses broches GPIO avec éventuellement utilisation d'entretoises pour bien le maintenir. On les trouve [ici](https://github.com/raspberrypi/hats/blob/master/hat-board-mechanical.pdf) sur la page GuitHub de Raspberry.
{: .text-justify}

A partir de ces spécifications on peut concevoir sous KICAD le format du PCB. Je ne suis d'ailleurs pas parti de zéro car on en trouve des tonnes déjà en libre service sur le net ;-) J'ai juste ajouté le logement d'un petit ventilateur 5v de taille 3cm * 3cm: si vous souhaitez concevoir un PCB sans ventilateur (il prend beaucoup de place...) c'est simple il suffira de le supprimer dans le modèle que je fourni.
{: .text-justify}

## Le modèle avec ventilateur sous KICAD

![HAT](/assets/images/tutos/004HAT/Kicad_HAT.png)
Le projet Kicad est disponible [ici](https://github.com/papsdroidfr/HAT_raspberry/blob/master/Kicad_HAT_FAN.zip), zippé sur ma page GitHub. Il faut partir de ce modèle pour fabriquer votre propre PCB. Les éléments sont verrouillés pour empêcher un malencontreux déplacement, mais ça n’empêche pas de déplacer ou supprimer des éléments: Kicad demande juste une confirmation car tout est verrouillé.
{: .text-justify}

Par exemple si vous n'avez pas besoin de l'énorme trou nécessaire au ventilateur, il suffit de le sélectionner avec ses 4 logements pour les vis, et de supprimer en confirmant.
{: .text-justify}

Pour alléger le schéma en enlevant toutes les annotations, il suffit de décocher sous Kicad la couche de dessins explicatifs "Dwgs.User" ainsi que la couche de commentaires "Cmts.USer".
{: .text-justify}

## Exemple pratique: le piOffHAT

Pour cette mise en pratique nous allons reprendre la base du tutoriel piOff auquel j'ajoute une gestion d'un ventilateur commandé par la température du CPU, et sous une tension que l'on peut switcher avec un interrupteur à bascule entre 5v et 3.3v.
{: .text-justify}

### Matériel nécessaire

![HAT](/assets/images/tutos/004HAT/20190921_121553_web.jpg)

Pour réaliser ce tutoriel nous aurons besoin de:
- 1 petit bouton poussoir 6mm type No
- résistances: 2*10kΩ + 1*1kΩ + 1*470Ω
- 1 led 3mm rouge
- 1 transistor NPN type to92
- 1 interrupteur à bascule coudé ON-ON, 3*pin, pas 2,54 mm
- 1 connecteur 2*pin mâle 2,54mm (pour y brancher le ventilateur)
- 1 barrette 2*20 pins femelle 2,54mm bas profil (4mm la hauteur du plastique, 7mm avec les pin)
- 4 entretoises M2.5 pour visser les pieds (5mm suffisent) et optionnel: 4 entretoises M2.5 8mm pour soutenir la carte (pas pris dans mon cas).
- 1 petit ventilateur 5v 3cm*3cm avec ses vis de fixation (optionnel: une grille de protection adaptée)
- Le PCB au format HAT: à faire fabriquer à partir des fichier GERBER fournis dans le fichier Kicad_piOffHAT.zip.
- Et bien sûr la vedette: son Raspberry pi3 ou 4 préféré


### Schéma électronique et routage

{% include gallery id="gallery1" caption="" %}
Rien de bien sorcier ni d'inhabituel si vous avez lu mes précédents tutoriels. Le bouton poussoir "Off" qui a pour fonction de provoquer l'extinction du Raspberry pi est connecté au pin10 du Raspberry, tandis que le ventilateur est commandé via un transistor par le pin 8 du Raspberry. Une led en parallèle du ventilateur va s'allumer quand le ventilateur est en marche. J'ai volontairement utilisé ces pin 8 et 10 afin de concentrer tout le routage de la carte vers la gauche et laisser l'autre partie à droite du ventilateur vierge: on peut ainsi repartir de ce projet pour le compléter avec vos propres idées ;-)
{: .text-justify}

### Programmes Python

un seul programme est à exécuter: piOffHAT.py qui est composé de quelques classes:
- **Classe Button_quit**: gère le bouton poussoir. Il est possible de l'instancier avec le paramètre powerOff=True (provoque l'extinction du Raspberry) ou False (n'éteint pas le Raspberry, mais sort du programme: pratique pour faire des tests)
{: .text-justify}
- **Classe ReadT**: il s'agit d'un Thread qui va lire toutes les 30s la température du CPU et activer ou désactiver en conséquence le ventilateur en envoyant des commandes HIGH(ventilateur On)/LOW(ventilateur Off) au pin8. 2 paramètres permettent de moduler la température de déclenchement (tFanMax) ou d'extinction (tFanMin) du ventilateur. Ne pas mettre de valeur trop proches sinon le ventilateur ne va pas cesser de s'éteindre/s'allumer/s'éteindre/s'allumer.... Par défaut, ces valeurs sont respectivement fixées à 55°C et 40°C.
{: .text-justify}
- **Classe Application**: c'est l'application principale qui va instancier un Button_quit ainsi qu'un ReadT qui démarre aussi sec. CTRL-C permet d'arrêter proprement le thread de lecture de la T°, et de quitter le programme. Un appui sur le bouton Off aura la mêle effet avec extinction du raspi si powerOff est à True (valeur par défaut). Un paramètre booléen supplémentaire "verbose" permet d'activer ou non les prints en mode console (pratique pour faire des tests). La boucle principale "loop" du programme ne fait rien (mise en veille d'1s en boucle infinie): c'est éventuellement ici que vous pouvez ajouter votre propre code si vous voulez compléter ce projet.
{: .text-justify}

Le projet est téléchargeable [sur ma page Github](https://github.com/papsdroidfr/HAT_raspberry).

![HAT](/assets/images/tutos/004HAT/20190921_144914_web.jpg)
