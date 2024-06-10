---
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
  teaser : "/assets/images/tutos/037ESP32Veilleuse/veilleuse_300.jpg"

#SEO tags
title       : "Veilleuse ESP32"
image       : "/assets/images/tutos/036ESP32meteo/boitier3D_300.jpg"
description : "Une veilleuse à base d'un ESP32 et d'un petit anneau de leds"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Veilleuse"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-pyhton" "électronique"
tags        : ["micro-python", "ESP32", "ruban leds"]

---

![Veilleuse](/assets/images/tutos/037ESP32Veilleuse/veilleuse_300.jpg){: .align-left}
Ce projet permet de construire une petite veilleuse sympathique imprimée 3D qui contrôle un petit anneau de 12 leds RGB 5v avec un ESP32. Cette veilleuse très sobre en énergie va consommer à peine 1.2 watts en fonctionnement, avec une animation au démarrage suivie d'un effet de léger crépitement comme un feu de bougie.
{: .text-justify}

## Hardware

### Matériel

Pour seul matériel, il faut:

* 1 [ESP32 MINI D1](https://www.az-delivery.de/fr/products/esp32-d1-mini) d'AZ-Delivery (tout autre ESP32 nécessitera d'adapter le socle de la veilleuse imprimé 3D)
* 1 petit anneau de [12 leds RGB WS2812B](https://www.az-delivery.de/fr/products/rgb-led-ring-ws2812-mit-12-rgb-leds-5v-fuer-arduino) 5V, 37mm  comme ceux qu'on trouve chez AZ-delivery 
* 3 petits câbles en fil dur à souder
* 1 alimentation 5v micro-usb avec bouton on/off: un modèle 15w pour alimenter un raspberry-pi 3 avec bouton on/off sera parfait.

rien de plus !

### assemblage

![Veilleuse](/assets/images/tutos/037ESP32Veilleuse/assemblage.jpg)
à l'aide de 3 fils à souder durs (pour maintenir l'anneau au dessus de l'ESP32 il est important d'utiliser des fils durs), reliez comme sur l'image:
{: .text-justify}

* VCC +5V de l'ESP32 au VCC de l'anneau
* GND de l'ESP32 au GND de l'anneau
* GPIO16 de l'ESP32 au IN de l'anneau

### Impression 3D

Le modèle de base à imprimer est disponible et gratuit sur Thingiverse. Il s'agit de cet [hibou rigolo](https://www.thingiverse.com/thing:3939396). J'ai retravaillé le socle afin de pouvoir y loger l'ESP32 au format carré ainsi que le cordon d'alimentation micro-usb: le socle de base ne le permet pas.
{: .text-justify}
* Imprimez-donc le hibou du modèle de base **avec un PLA translucide**. Je conseille une hauteur de couche à 0,15mm pour avoir une belle définition des détails. L'usage des supports est obligatoire. un PLA translucide permettra de laisser passer la lumière.
{: .text-justify}

* Imprimez ensuite le socle modifié que je mets à dispostion sur mon [Github](https://github.com/papsdroidfr/ESP32_Veilleuse), dans la section **/STL**. une hauteur de couche en 0.3mm est suffisante pour le socle, et pas besoin de support.
{: .text-justify}

![Veilleuse](/assets/images/tutos/037ESP32Veilleuse/socle.jpg)

## Software

### configuration ESP32

Il faut dans un premier temps flasher la dernière version de micro-python sur l'ESP32 à l'aide de l'IDE [Thonny](https://thonny.org/). L'opération est très simple et se passe en 4 temps:
{: .text-justify}
* téléchargez la dernier **firmware** (c'est un fichier .bin) de [micro-python pour ESP32](https://micropython.org/download/ESP32_GENERIC/) sur le site officiel micro-python. Pas besoin d'installer esptool , Thonny sait comment flasher un firmware sur un ESP32 relié par USB sur l'ordinateur. A l'heure où j'écris ces lignes, j'ai téléchargé la version v1.22.2(2024-02-22).bin
{: .text-justify}
* Ouvrez Thonny, et connectez l'ESP32 à l'ordinateur  à l'aide d'un cable USB.
{: .text-justify}
* Allez dans Outils -> Options -> Interpréteurs et choisir "MicroPython (ESP32)", puis cliquer sur "Installer ou mettre à jour le firmware"
{: .text-justify}
* choisir le port dans la liste déroulante (normalement le bon port est reconnu dès le branchement de l'ESP32), puis sélectionnez l'image .bin téléchargée sur votre ordinateur.
{: .text-justify}
Après une minute ou deux de mise à jour, ça y est le flash micropython sur l'ESP32 est prêt.
{: .text-justify}

### script micropython

Le script principal **main.py** est à récupérer sur le [Github du projet](https://github.com/papsdroidfr/ESP32_Veilleuse) dans la section **/micropython**. Il va lancer une petite animation qui dure quelques secondes et ensuite basculer sur une couleur avec un petit effet de crépitement.
{: .text-justify} 

Si vous souhaitez modifier la couleur (dans mon script j'utilise du violet = 50% bleu  + 50% rouge) il suffit d'adapter la ligne 39 

```python
        for j in range(n):
            np[j] = (128+delta, 0, 128+delta)
```
la varialbe **delta** sert à simuler le crépitement. (128, 0, 128) représente du violet: mettez ce que vous voulez comme couleur sans oublier d'y rajouter +delta pour simuler le crépitement. Par exemple si vous voulez du vert: (0,128+delta,0) fera l'affaire. Ou bien (0,0,128+delta) pour du bleu, ou encore (0, 128+delta, 128+delta) pour obtenir du jaune (vert + bleu) etc ... On peut simuler 65 millions de couleurs comme ça.
{: .text-justify} 

{% include video id="YkUEj5xpYDk" provider="youtube" %}
