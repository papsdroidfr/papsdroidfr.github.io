---
#source: https://www.papsdroid.fr/post/tutoriel-sysdroid-hat
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
  teaser : "/assets/images/tutos/003sysdroid/03sysdroidHAT/sysdroidHAT_300.jpg"

#SEO tags
title       : "sysdroidHAT"
image       : "/assets/images/tutos/003sysdroid/03sysdroidHAT/sysdroidHAT_300.jpg"
description : "carte HAT pour monitoring raspberry pi 3"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Sysdroid HAT"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pi" ,  "python3", "électronique", "fan", "HAT", "monitoring"]

gallery1:
  - image_path: "/assets/images/tutos/003sysdroid/03sysdroidHAT/Gerber_top.png"
  - image_path: "/assets/images/tutos/003sysdroid/03sysdroidHAT/Gerber_verso.png"

---
![SysdroidHAT](/assets/images/tutos/003sysdroid/03sysdroidHAT/sysdroidHAT_300.jpg){: .align-left} 
Troisième version de mon projet de monitoring système d'un Raspberry pi: le **Sysdroid HAT**. Il s'agit d'un monitoring temps réel du CPU, de la RAM, de l'espace disque, de la température avec contrôle automatique d'un ventilateur 
<!--more-->
et affichage des informations sous forme de barres verticales de % et de messages scrollés sur une matrice de 8*8 Leds, avec en prime un bouton d'extinction Off général, le tout cette fois sur un circuit imprimé qui respecte le standard du format HAT comme on peut le voir sur la photo.
{: .text-justify}

Rien de mieux qu'une vidéo pour présenter ce sysdroid HAT:
{% include video id="C7BMiRvzhHo" provider="youtube" %}

## Conception du circuit imprimé

![SysdroidHAT](/assets/images/tutos/003sysdroid/03sysdroidHAT/kicad_schema.png){: .align-center} 
{% include gallery id="gallery1" caption="circuit imprimé" %}

Le schéma électronique ainsi que le matériel nécessaire sont identiques à ma première version du sysdroid que j'ai longuement expliqué dans cet article. Quand au routage du circuit imprimé en respectant le format HAT 65*56mm avec en prime un énorme trou pour le ventilateur: ça été un véritable casse tête, mais possible, entièrement conçu sous Kicad depuis un Raspberry pi4 4Go.
{: .text-justify}

A noter que le couplage d'un ventilateur avec un transistor tel que je l'ai décrit dans le matériel nécessaire (article cité plus haut) nécessite un point d'attention: je me suis rendu compte que ça ne fonctionne pas avec n'importe quel ventilateur. En effet les transistor TO92 sont limité à 0,85W de puissance: donc les ventilateurs 5v 0,2A (= 1W) ne pourront pas démarrer: il faut un ventilateur 5v avec 7 pales qui fait 0,1A: là non seulement ça marche mais en plus ce ventilateur qui consomme peu est totalement silencieux contrairement aux modèles 0,2A et plus qui tournent beaucoup trop vite et font un boucan de tous les diables.
{: .text-justify}

## Programmes Python3

Pour réaliser ce circuit, vous pouvez télécharger sur [ma page Github](https://github.com/papsdroidfr/sysdroidHAT) les fichiers GERBER zippés correspondant au circuit imprimé, et les 2 programmes pythons nécessaires au fonctionnement:
{: .text-justify}

**stdLedMatrix.py**: contient la classe **LedMatrix** qui va gérer tout l'affichage sur la matrice de leds sous forme de Thread: affichage de logo, allumage/extinction de leds précises, scrolling de messages textes, animations diverses. 
{: .text-justify}

**sysdroidHAT.py**: programme principal à lancer. Il contient 3 classes
- **Readsys**: thread de lecture des informations systèmes (CPU, RAM, espace disque, température et contrôle du ventilateur), prépare les affichages en tâche de fond pour la matrice de leds.
{: .text-justify}
- **Sysdroid**: encore un thread qui gère la carte: gestion des boutons poussoirs Off (provoque l'extinction du Raspberry pi) et Suivant (changement d'affichage de la matrice de leds), affichage de logo de démarrage et d'animations diverses.
{: .text-justify}
- **Application**: l'application principale qui peut être instanciée avec plusieurs paramètres utiles: tFanMin (température qui provoque l'arrêt du ventilateur: 42°C par défaut), tFanMax (température qui provoque le démarrage du ventilateur: 55°C par défaut), delay (temps d'attente en secondes avant recyclage des données: 1s par défaut), speedscroll (agit sur la vitesse de défilement des messages scrollés: 0.1s par défaut), inverse(possibilité d'inverser l'affichage à 180°: False par défaut), verbose (active des affichages en console: False par défaut).
{: .text-justify}

Ce système consomme à peine 3% à 4% des CPU et permet de faire descendre très rapidement la température du CPU de plus de 10°C dès que les 58°C sont atteints, et ce en totale discrétion avec un ventilateur 7 pales 0,1A: c'est inaudible. Le circuit consomme malgré tout plus que la version [sysdoid_UHHD](https://papsdroidfr.github.io/tutoriels/sysdroid-unicorn-hat/) avec la matrice de 256 Leds: c'est normal car ici je dois gérer de manière logicielle l'affichage sur la matrice de leds, tandis que la Unicorn HAT HD est dotée d'un processeur dédié ARM qui s'en charge. Les seules différences entre ces deux versions de sysdroid c'est que dans un cas on peut ajouter un ventilateur et j'ai un bouton d'extinction Off, et dans l'autre: pas de ventilateur ni de bouton off, mais un affichage en couleur grand format du plus bel effet, que j'ai privilégié par exemple sur [mon serveur NAS](https://papsdroidfr.github.io/configuration/raspi3nas/).
{: .text-justify}