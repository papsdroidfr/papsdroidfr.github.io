---
#source: https://www.papsdroid.fr/post/picotimer
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
  teaser : "/assets/images/tutos/032PicoTimer/picotimer-300.jpg"

#SEO tags
title       : "Compte à rebours PicoTimer"
image       : "/assets/images/tutos/032PicoTimer/picotimer-300.jpg"
description : "Ce projet concerne un compte à rebours contrôlé par un Raspberry PICO. Il est doté d'un écran LCD, d'un buzzer , de 4 boutons poussoirs et d'une led RGB"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "PicoDisplay"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pico" , "micro-python", "Pimoroni", "multi-thread"]

gallery1:
  - image_path: "/assets/images/tutos/032PicoTimer/hardware1.jpg"
  - image_path: "/assets/images/tutos/032PicoTimer/hardware2.jpg"

gallery2:
  - image_path: "/assets/images/tutos/032PicoTimer/3D_01.png"
  - image_path: "/assets/images/tutos/032PicoTimer/3D_02.png"
  - image_path: "/assets/images/tutos/032PicoTimer/3D_03.png"

---
![PicoDisplay](/assets/images/tutos/032PicoTimer/picotimer-300.jpg){: .align-left}
Ce projet concerne un compte à rebours contrôlé par un Raspberry PICO. Il est doté d'un écran LCD, d'un buzzer , de 4 boutons poussoirs et d'une led RGB. Le timer se règle de 1s à 90mn. Lorsqu'il démarre, la led RGB clignote en vert, puis clignote en rouge s'il reste moins de 30' avec le buzzer qui bipe. Enfin s'il reste moins de 10 secondes le clignotement rouge s'accélère. Lorsque le timer est terminé, le buzzer sonne plus aigüe.
{: .text-justify}
* En haut à gauche: bouton Init, sert à initialiser le timer à zéro lors d'un appui long
* En haut à droite: bouton Start pour démarrer le timer
* En bas à gauche: réglage des minutes (appuis courts et longs possibles)
* En bas à droite: réglage des secondes (appuis courts et longs possibles)

## Matériel nécessaire
* 1 Raspberry PICO
* 1 buzzer passif
* 1 PICO Display 1.14" de Pimoroni (avec écran LCD, 1 led RGB et 4 boutons poussoirs intégrés)
* Une alimentation 5v mini usb (type Raspberry pi3)
* (optionnel) 1 boîtier imprimé 3D

## Configuration hardware
{% include gallery id="gallery1" caption="" %}
Il faut dans un premier temps souder des header sur le PICO, attention les soudures doivent se faire **par dessus**: les pin ressortent donc sous le PICO (voir première image). Le buzzer passif peut se souder directement sur les pin0 et pin3 du PICO. Ne pas les souder trop à ras-bord du PICO car il faut laisser un peu d'espace pour le boîtier imprimé 3D.
{: .text-justify}

Le PICO display se loge directement sur les pin header par dessous le PICO donc. le logo pico display ainsi que les boutons A et B doivent être orientés vers l'USB du PICO.
{: .text-justify}

## Configuration Software
Pour configurer le PICO afin d’interagir avec cette fabuleuse petite carte pico display: lisez [cet article](https://papsdroidfr.github.io/tutoriels/picodisplay/). Le programme micropython est à récupérer sur le [Github du projet](https://github.com/papsdroidfr/PicoTimer), dans la section **/micropython**.
{: .text-justify}

Ce programme exploite les capacités multi-thread du PICO qui fonctionne avec 2 cœurs. Pour une meilleure compréhension je vous encourage à lire [cet article](https://papsdroidfr.github.io/dev/PicoMultiThread/) dédié sur le sujet. J'utilise ainsi le cœur principal du PICO pour gérer la séquence d’interaction avec l'utilisateur (boucle principale qui attend que le timer soit initialisé, puis se lance quand l'utilisateur appuie sur le bouton start, et attend que le timer se temrine ou bien que l'utilisateur fasse un appui long sur le bouton Init). Le coeur secondaire du PICO quand à lui est exclusivement dédié à l'affichage des informations sur l'écran LCD, le clignotement d ela led et l'activation du buzzer.
{: .text-justify}

## Impression 3D
{% include gallery id="gallery2" caption="" %}
Pour imprimer le boitier vous pouvez prendre les STL disponible sur le [Github du projet](https://github.com/papsdroidfr/PicoTimer) dans la section **/STL**. Il y a une partie basse à imprimer et une partie haute. Pensez à activez les supports pour la partie basse, à cause du passage USB. Pour avoir un effet "bicolore" comme sur la photo principale en titre, il suffit de programmer une pose à mi-hauteur pour changer de bobine, puis une seconde pause 15 ou 20 couches plus haut pour remettre la 1ère couleur: cela créé le liseré central. Dans mon cas j'ai utilisé un filament jaune Or et le liseré en Bronze. 
{: .text-justify}

>Et voilà c'est tout simple comme projet. Plus qu'à lancer un Timer de 3mn: pile-poil pour aller se faire cuire un œuf :-)
{: .text-justify}