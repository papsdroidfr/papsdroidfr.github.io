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
  teaser : "/assets/images/tutos/04116bitsParallelCard/carte_300.jpg"

#SEO tags
title       : "Interface parallèle 16bits pour 8 moteurs pas à pas."
image       : "/assets/images/tutos/04116bitsParallelCard/carte_300.jpg"
description : "Interface parallèle 16bits pour pilotage de 8 moteurs pas à pas"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Interface 16bits//"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "DEV" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-python" "électronique"
tags        : ['micro-python', 'raspberry pico', 'stepper-motor', 'TMC2208', 'TMC2209', 'A4988']

gallery_montage_PCB:
  - url: /assets/images/tutos/04116bitsParallelCard/PCB_montage_1024.jpg
    image_path: "/assets/images/tutos/04116bitsParallelCard/PCB_montage_800.jpg"
    title: "PCB assemblé."
---

![16bits](/assets/images/tutos/04116bitsParallelCard/carte_300.jpg){: .align-left}
Cette carte est une interface parallèle 16bits qui peut piloter jusqu'à 8 moteurs pas à pas (Stepper Motor) en mode step/dir à l'aide d'un fichier de trajectoires pré-calculées (Gcode file) au format json. Les moteurs peuvent être drivés avec au choix des A4988 (bruyants et dépassés), ou bien mieux avec des TMC2208 ou TMC2209 qu'il faudra configurer en mode step/dir et non pas UART. L'interface parallèle est gérée par une machine à état hébergée sur un Raspberry-pico. Elle va servir à animer des automates nécessitant jusqu'à 8 moteurs.
{: .text-justify}

## Hardware

### Matériel nécessaire

Il vous faut:

- 1 **Raspberry PICO**, toutes les versions 1 ou 2 sont compatibles.
{: .text-justify}
- 1 petite **diode Schottky** 5v 1A, au format DO-41.
- 1 à 8 moteurs pas à pas **12v** de type **NEMA17** (j'utilise  des modèles NEMA17HE15-1504S)
{: .text-justify}
- 1 à 8 **drivers** de moteurs A4988(je ne recommande pas: ils sont très bruyants et peu fiables), ou bien **TMC2208** en mode step/dir ou encore **TMC2209** en mode step/dir. **Attention** à ne pas prendre des TMC2208 ou TMC2209 en mode UART: ils ne sont pas compatibles avec cette carte.
{: .text-justify}
- Une alimentation **DC 12v** de 360 Watts si vous comptez utiliser 8 moteurs.
{: .text-justify}
- 1 à 8 **micro-switch DIP** avec 3 boutons, pour configurer les micro-steps de chaque moteur.
{: .text-justify}
- 9 **condensateurs** 25v 100uf, les plus petits en hauteur (il faut les loger sous les drivers TMC)
{: .text-justify}
- 1 **led 5mm** pour visualiser la mise sous tension de la carte (j'ai opté pour une led rouge)
{: .text-justify}
- 1 **résistance** 1/4watts de 220ohms (pour la led)
{: .text-justify}
- 1 convertisseur 12v-5v **TRACO TSR**: leur rendement est excellent.
{: .text-justify}
- 1 connecteur mâle 12v **XT60**
{: .text-justify}
- 1 **pin header mâle 4 pins** pour connecter l'interface i2c (pour un écran OLED déporté)
{: .text-justify}
- 1 **pin header mâle 2 pins** pour connecter un ventilateur 12v
{: .text-justify}
- 1 à 8 **connecteurs mâles 4 pins** pour brancher les moteurs
{: .text-justify}
- 1 connecteur **JST mâle 3 pins** pour brancher une interface UART
{: .text-justify}
- 2 à 16 **pins header femelles 8 pins** afin d'y connecter les 1 à 8 drivers sans avoir à les souder sur la carte.
{: .text-justify}
- 2 **pins headers femelles 20 pins** afin d'un connecter le raspberry pico sans avoir à le souder sur la carte.
{: .text-justify}
- 1 ventilateur 12v 60mm: je recommande un **Noctua NF-A6x15 FLX** car il est très silencieux.
- bien entendu **la carte PCB** que vous pouvez re-faire fabriquer à partir des fichiers GERBER fournis dans [ce Github](https://github.com/papsdroidfr/Stepper16bitsControl) dans le dossier **hardware**, ou bien vous pouvez me contacter pour voir s'il men reste en stock.
{: .text-justify}

### Assemblage de la carte

![16bits](/assets/images/tutos/04116bitsParallelCard/PCB_800.jpg){: .align-center}
Si j'ai bien compté, il y a 333 soudures à faire sur cette carte. Les sérigraphies sont assez claires, il faut juste veiller à bien souder les condensateurs dans le bon sens (la patte la plus longue sur le +), ainsi que la led (idem patte la plus longue sur le +) et la diode Schottky dont la cathode est sérigraphiée sur le PCB. Les switch DIP doivent aussi être soudés comme indiqués sur la sérigraphie ("On" vers le "1").
{: .text-justify}

![16bits](/assets/images/tutos/04116bitsParallelCard/PCB_soudures_800.jpg){: .align-center}
Pour souder les pins header femelle, je conseille vivement de les souder par paire avec les driver en place (idem pour les pins header femelle du Raspberry-PICO à souder avec le PICO en place): ceci afin de les souder bien droits.
{: .text-justify}

{% include gallery id="gallery_montage_PCB" caption="Cliquez pour agrandir l'image." %}
Placez ensuite le Raspberry-pico dans le sens indiqué par la sérigraphie (prise usb vers le haut). Les drivers sont tous orientés dans le même sens, avec la petite vis de réglage vers la gauche comme sur la photo.
{: .text-justify}

### Impression 3D boîtier

![16bits](/assets/images/tutos/04116bitsParallelCard/boitier3D_300.png){: .align-left}
Un boîtier à imprimer 3D est à disposition dans le [Github du projet](https://github.com/papsdroidfr/Stepper16bitsControl) dans le dossier **3D_STL**.
Il y a une partie basse dans laquelle la carte se loge, et une partie haute qui permet de maintenir en place le ventilateur et le passage des câbles (alimentation 12v et moteurs). La partie haute est à imprimer à l'envers: face haute vers le bas, pour éviter de faire trop de supports.
{: .text-justify}
![16bits](/assets/images/tutos/04116bitsParallelCard/boitier3D_600.png){: .align-center}

### Connection des moteurs

![16bits](/assets/images/tutos/04116bitsParallelCard/NEMA17_600.png){: .align-center}

J'ai pris en référence un **NEMA17HE15** qui a un connecteur 4 fils de 4 couleurs (Noir, Vert, Bleu, Rouge) avec 4 références qu'on retrouve sur chaque connecteur sérigraphié sur la carte (A+, B+, A-, B-). Si les branchements ne sont pas respectés en fonction du moteur choisi, il va vibrer au lieu de tourner correctement.
{: .text-justify}
- Noir: A+,
- Vert: B+,
- Bleu: A-,
- Rouge: B-

> **Attention** les moteurs doivent se brancher ou débrancher uniquement lorsque la carte est **hors tension**, sinon vous risquez de les détruire ainsi que les drivers.
{: .text-justify}

Chaque moteur sur la carte est sérigraphié de **M0** à **M7**.
{: .text-justify}

Pour brancher le ventilateur il suffit de brancher les 2 fils +12v et Gnd sur l'emplacement adéquat sur la carte. Si vous avez un ventilateur 3 fils comme avec le **Noctua NF A6-X15**, repérez bien le fil rouge (+12v) et le fil noir (Gnd). Quand au fil jaune qui sert à mesurer la vitesse du ventilateur: vous pouvez le laisser débranché, la carte ne l'exploite pas.
{: .text-justify}

### Micro-stepping des moteurs

Le **micro-stepping** de chaque moteur (full, 1/2pas, 1/4pas, 1/8pas etc ...) est défini à l'aide des DIP micro-switch: 3 positions on/off sont nécessaires pour un A4988, mais seuls les 2 1er on/off seront nécessaires pour les TMC2208 et TMC2209. Suivez les instructions de votre driver pour paramétrer correctement le micro-stepping souhaité.
{: .text-justify}

> **Attention**: ne pas modifier les positions des DIP-SWITCH lorsque les moteurs sont en train de fonctionner: il faut le faire hors tension.
{: .text-justify}

## Software

Le firmware de la carte en **micro-python** est conçu pour fonctionner avec un **Raspberry PICO**, version 1 ou 2 peut importe. L'interface parallèle 16 bits est gérée par une machine à état.
{: .text-justify}

### Micro-python

Si vous installez **micro-python** pour la première fois sur un **Raspberry PICO**, [cet article](https://papsdroidfr.github.io/configuration/pico/) en résume les étapes.
{: .text-justify}

### Firmware

Le firmware écrit en **micro-python** est à récupérer sur le [Github du projet](https://github.com/papsdroidfr/Stepper16bitsControl) dans la section **software**. Il faut bien veiller à récupérer tous les modules.
{: .text-justify}

- **gcode**: contient les fichiers de trajectoire en exemple, au format json.
{: .text-justify}
- **sm**: module qui gère la machine à état pour l'interface parallèle 16 bits.
{: .text-justify}
- **stepper**: module de gestion des drivers.
{: .text-justify}
- **main_automat.py**: programme principal qui lance une animation parmi les fichiers de trajectoires qu'il y a sur la carte. Pour une exécution automatique au branchement de la carte, il suffit de **renommer ce fichier en main.py**. Sinon il faudra relier le Raspberry PICO à l'ordinateur via la prise USB pour démarrer le programme.
{: .text-justify}
