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
  teaser : "/assets/images/tutos/044_1motor/pcb4_300.png"

#SEO tags
title       : "Stepper motor TMC"
image       : "/assets/images/tutos/044_1motor/pcb4_300.png"
description : "PCB pour contrôler un moteur pas à pas avec driver TMC"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Stepper Motor"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-python" "électronique"
tags        : ["micro-python", "raspberry pico", 'stepper-motor', 'TMC2208', 'TMC2209']

gallery_schema:
  - url: /assets/images/tutos/044_1motor/kicad_800.png
    image_path: "/assets/images/tutos/044_1motor/kicad_600.png"
    title: "Schéma KICAD"

gallery_pcb:
  - url: /assets/images/tutos/044_1motor/pcb2_600.png
    image_path: "/assets/images/tutos/044_1motor/pcb2_300.png"
    title: "premières soudures"
  - url: /assets/images/tutos/044_1motor/pcb3_600.png
    image_path: "/assets/images/tutos/044_1motor/pcb3_300.png"
    title: "Montage final"


---

![Stepper](/assets/images/tutos/044_1motor/pcb4_300.png){: .align-left}
Cette carte permet de contrôler un moteur pas à pas à l'aide d'un TMC2208 ou TMC2209 avec comme software le système d'interface parallèle 16bits utilisé dans [ce projet de contrôle de 8 moteurs](https://papsdroidfr.github.io/dev/16bitsParallelControlCard/). Le contrôle du moteur pour le faire avancer ou reculer à des vitesses précises passe par un fichier gcode, écrit en json.
{: .text-justify}

## Hardware

### Matériel nécessaire

Il vous faut:

- Un **Raspberry PICO**, version 1 ou 2 peu importe.
{: .text-justify}
- Un moteur pas à pas 12v de type **NEMA17** (personnellement j'utilise  un modèle 17HE15-1504S)
{: .text-justify}
- Un **driver TMC2208 ou TMC2209** qui va permettre de convertir les instructions en 3.3v envoyées par le microcontrôleur en 12v vers les bobines du moteur pas à pas. Ces drivers sont miniatures, efficaces et permettent un contrôle du moteur sans aucun bruit. La carte fonctionnera aussi avec un  driver A4988, mais ils sont dépassés, et bien moins efficaces que les drivers TMC.
{: .text-justify}
- Une alimentation **DC 12v 60Watts (5A)** avec un connecteur Jack DC. Attention à ne pas prendre une alimentation 5v.
{: .text-justify}
- 1 **micro-switch DIP** avec 3 boutons, pour configurer les micro-steps de chaque moteur. Seuls les 2 premiers boutons sont utiles pour les drivers TMC (le troisième ne sert que pour les driver A4988).
{: .text-justify}
- 1 **condensateurs** 25v 100uf, les plus petits en hauteur (il faut les loger sous les drivers TMC).
{: .text-justify}
- 1 **led 5mm** pour visualiser la mise sous tension de la carte (j'ai opté pour une led rouge).
{: .text-justify}
- 1 **résistance** 1/4watts de 220ohms (pour la led).
{: .text-justify}
- 1 convertisseur 12v-5v **TRACO TSR**: leur rendement est excellent (>90%).
{: .text-justify}
- 1 connecteur **JackDC** à souder avec 3 pattes.
{: .text-justify}
- 2 **pin header mâle 4 pins** pour connecter le moteur et une interface i2c optionnelle (pour un écran OLED déporté).
{: .text-justify}
- 2 **pins header femelles 8 pins** afin d'y connecter le driver sans avoir à les souder sur la carte.
{: .text-justify}
- 2 **pins headers femelles 20 pins** afin d'y connecter le raspberry pico sans avoir à le souder sur la carte.
{: .text-justify}
- bien entendu **la carte PCB** que vous pouvez re-faire fabriquer à partir des fichiers GERBER fournis dans [ce Github](https://github.com/papsdroidfr/pcb_1_stepper_motor) dans le dossier **PCB**, ou bien vous pouvez me contacter pour voir s'il men reste en stock.
{: .text-justify}

### Schéma électronique

{% include gallery id="gallery_schema" caption="Cliquez pour agrandir l'image." %}

### Assemblage de la carte

![Stepper](/assets/images/tutos/044_1motor/pcb1_600.png){: .align-center}
Aucune difficulté, il ne faut pas se tromper de sens avec la soudure de la diode (Cathode matérialisée avec une petite barre horizontale à souder sur le K), de la led (patte la plus courte sur la masse), du condensateur (patte la plus courte sur la masse) et du convertisseur 12v-5v: la sérigraphie aide à se repérer. Connectez dans le bon sens aussi le Raspberry PICO (port usb dans la même direction que la prise Jack) et le driver TMC (vis de réglage vers l'extérieur de la carte), sinon ils seraient détruits dès la mise sous tension du système. Le micro-dip switch sérigraphié est en position OFF.
{: .text-justify}
{% include gallery id="gallery_pcb" caption="Cliquez pour agrandir les images" %}
{: .text-justify}
Le moteur se branche sur les pin header localisés à côté de l'alimentation Jack DC. 
{: .text-justify}


### Boîtier imprimé 3D

![Stepper](/assets/images/tutos/044_1motor/boitier_600.png){: .align-center}

Un boîtier minimaliste à imprimer en 3D est disponible dans la section **/STL** du [Github](https://github.com/papsdroidfr/pcb_1_stepper_motor) du projet.

## Software

### Installation bibliothèque

Installez bien la dernière version MicroPython sur votre microcontrôleur. Cet [article](https://papsdroidfr.github.io/configuration/pico/) détaille comment s'y prendre avec un Raspberry PICO.
{: .text-justify}

Tout le code micropython est à récupérer sur le [Github du projet](https://github.com/papsdroidfr/pcb_1_stepper_motor) dans la section **micropython**.
{: .text-justify}

Tous les modules sont à uploader à la racine du PICO.
{: .text-justify}
- **/gcode**: fichier gcode au format json
{: .text-justify}
- **/sm**: module de gestion de la machine à état qui gère l'interface parallèle 16bits.
{: .text-justify}
- **/stepper**: module qui gère le driver TMC (ou A4988).
{: .text-justify}
- **main.py**: progamme principal qui s'exécute au démarrage du système.
{: .text-justify}


### Usage

Le pilotage du moteur se fait grâce au fichier gcode déposé dans le dossier **/gcode**. Pour comprendre comment écrire un fichier gcode pour ce système, je vous encourage à lire la section **fichier de trajetoire** de [cet article](https://papsdroidfr.github.io/dev/16bitsParallelControlCard/#fichiers-de-trajectoires).
{: .text-justify}

![Stepper](/assets/images/tutos/044_1motor/pcb_montage_600.jpg){: .align-center}
