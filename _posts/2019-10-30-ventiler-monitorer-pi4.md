---
#source: https://www.papsdroid.fr/post/ventiler-et-monitorer-son-raspberry-pi4
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
  teaser : "/assets/images/tutos/006VentilaPi4/UHHD/ventilapi4UHHD_300.jpg"

#SEO tags
title       : "Monitorer et ventiler son Raspberry pi4"
image       : "/assets/images/tutos/006VentilaPi4/UHHD/ventilapi4UHHD_300.jpg"
description : "Monitoring et ventilateur silencieux made in France (Garatorinic) pour Raspberry pi 4"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "VentilaPi4"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "configuration" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pi" , "fan", "HAT", "Pimoroni", "matrice leds", "monitoring"]

gallery1:
  - image_path: "/assets/images/tutos/006VentilaPi4/UHHD/20191029_185333_web.jpg"
  - image_path: "/assets/images/tutos/006VentilaPi4/UHHD/20191030_113953_web.jpg"

gallery2:
  - image_path: "/assets/images/tutos/006VentilaPi4/UHHD/20191030_114038_web.jpg"
  - image_path: "/assets/images/tutos/006VentilaPi4/UHHD/20191030_114244_web.jpg"
---

![SysdroidHAT](/assets/images/tutos/006VentilaPi4/UHHD/ventilapi4UHHD_300.jpg){: .align-left} 
Ventiler et Monitorer en temps réel son Raspberry pi4, sans utiliser de fer à souder: c'est possible en combinant l'utilisation d'un [VentilaPi4](https://papsdroidfr.github.io/configuration/ventilapi4/) de Garatronic et le système de monitoring temps réel [sysdroid_UHHD](https://papsdroidfr.github.io/tutoriels/sysdroid-unicorn-hat/) qui utilise une matrice de Leds Unicorn Hat HD. 
{: .text-justify}

## Matériel nécessaire:
![SysdroidHAT](/assets/images/tutos/006VentilaPi4/UHHD/20191030_113857_web.jpg){: .align-center} 
- Un **Raspberry pi4** fonctionnel et toute sa connectique, (boitier Pimoroni Ninja optionnel)
{: .text-justify}
- Le ventilateur [VentilaPi4](https://shop.mchobby.be/fr/raspberry-pi-4/1659-ventillateur-silencieux-pour-raspberry-pi-3232100016590-garatronic.html) de Garatronic (environ 12€, hors frais de ports)
{: .text-justify}
- La matrice 16*16 Leds [Unicorn HAT HD](https://shop.pimoroni.com/products/unicorn-hat-hd). (On la trouve pour environ 37€ sur Amazon)
{: .text-justify}
- un [connecteur 2*20 pins, pas 2.54 mm, femelle, bas profil](https://www.lextronic.fr/connecteur-femelle-2x20-bas-profil-21041.html) (hauteur: 5mm).
{: .text-justify}
- optionnel: **4 entretoises M2.5 15mm mâle-femelle**. Elles sont optionnelles car la matrice de leds est maintenue juste au dessus des grosses vis banches du ventilateur: aucun risque de court circuit car les vis sont en plastiques.
{: .text-justify}

{% include gallery id="gallery2" caption="" %}

- Bien placer dans un premier temps la broche traversante du Ventilapi4 par dessous le ventilateur: les pin mâles doivent ressortir de l'autre côté, au dessus donc.
{: .text-justify}
- Placer le connecteur bas profil 2*20 pin femelles directement sur les broches de la matrice de leds: bien enfoncer.
{: .text-justify}
- optionnel: enlever les 4 écrous qui maintiennent la carte du VentilaPi4, et y placer les 4 entretoises 15mm M2.5 à la place des écrous.
{: .text-justify}
- Brancher la matrice de leds sur les connecteurs mâle qui dépassent du VentilaPi4: si vous avez postionné les entretoises 15mm elles arrivent pile poil sous les trous de fixation de la matrice de leds. Dans se cas il faut enlever les 4 écrous sous la matrice qui maintiennent le diffuseur en plexiglass, et visser directement les vis qui sont compatibles avec les entretoises M2.5
{: .text-justify}

>Et voilà le tour est joué: pas de fer à souder!


## Installation du monitoring temps réel:

Vous pouvez pas à pas suivre [ce tutoriel](https://papsdroidfr.github.io/tutoriels/sysdroid-unicorn-hat/), il y a quelques explications sur le fonctionnement des programmes Python3 qui gèrent le monitoring.
{: .text-justify}

Si vous voulez que le système de monitoring démarre dès le démarrage du Raspberry pi, en supposant que vous avez placés les 2 programmes pythons (**sysdroid_UHHD.py** et **sysdroid_main.py**) dans **/home/pi/Python/**, rien de plus simple (sous os Raspbian): dans un terminal de commande, tapez:
{: .text-justify}

```
sudo nano /etc/rc.local
```

cela va ouvrir le fichier de configuration des démarrages automatiques
{: .text-justify}

juste avant la ligne "exit 0" à la fin de ce fichier, il faut ajouter cette ligne:
{: .text-justify}

```
python3 "/home/pi/Documents/Python/sysdroid_main.py" &
```

- le caractère & à la fin est très important: il indique d'exécuter ce programme python en tâche de fond, sans quoi le système serait bloqué à ne faire que ça...
{: .text-justify}
- Ctrl-O pour sauvegarder le fichier (confirmer par Oui) et  CTRL-X pour sortir.
{: .text-justify}

pour tester avant le reboot: 

```
sudo /etc/rc.local 
```
va exécuter les commandes du fichier: le monitoring doit démarrer.

> Au prochain reboot: il démarrera systématiquement.


## Petite vidéo récapitulative
{% include video id="TxHeYtr8Xes" provider="youtube" %}