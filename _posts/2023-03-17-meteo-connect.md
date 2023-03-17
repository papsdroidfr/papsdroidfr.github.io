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
  teaser : "/assets/images/tutos/036ESP32meteo/boitier3D_300.png"

#SEO tags
title       : "Meteo Connect"
image       : "/assets/images/tutos/036ESP32meteo/boitier3D_300.png"
description : "Météo de salon connectée pilotée par un ESP32-C3"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "MeteoConnect"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-pyhton" "électronique"
tags        : ["pybstick", "micro-python", "ESP32", "écran LCD", "DHT22"]

gallery1:
  - image_path: "/assets/images/tutos/036ESP32meteo/assemblage1.jpg"
  - image_path: "/assets/images/tutos/036ESP32meteo/assemblage2.jpg"
  - image_path: "/assets/images/tutos/036ESP32meteo/assemblage3.jpg"

gallery2:
  - image_path: "/assets/images/tutos/036ESP32meteo/ecran1.jpg"
  - image_path: "/assets/images/tutos/036ESP32meteo/ecran2.jpg"
  - image_path: "/assets/images/tutos/036ESP32meteo/ecran3.jpg"

---

![MeteoCOnnect](/assets/images/tutos/036ESP32meteo/boitier3D_300.png){: .align-left}
Ce projet détaille pas à pas la construction d'une petite station météo de salon connectée, à base d'un [EPS32-C3](https://papsdroidfr.github.io/configuration/Pybstick-C3/). Un capteur de température interne DHT22 permet d'afficher la température et l'humidité de la pièce dans laquelle il fonctionne, tandis que les données météorologiques externes sont collectées sur internet en WIFI par un appel d'API. Le tout est affiché sur un petit écran LCD de 2 lignes par 16 caractères, et tient dans un petit boîtier à imprimer en 3D.
{: .text-justify}

## Hardware

### Matériel

![MeteoCOnnect](/assets/images/tutos/036ESP32meteo/materiel.jpg){: .align-center}

pour ce projet il faut:

* 1 [PYBSTICK ESP32-C3](https://shop.mchobby.be/fr/pybstick/2505-pybstick26-esp32-c3-micropython-et-arduino-3232100025059.html) micro-usb avec les 2 rangées de pin headers 13 broches soudées. Cet ESP32-C3 est fabriqué en France.
{: .text-justify}
* 1 PCB que vous pouvez faire fabriquer à partir des fichier Gerber fourni dans la section **/PCB** du [Github du projet](https://github.com/papsdroidfr/MeteoConnect), ou bien me contacter pour savoir s'il m'en reste en stock.
{: .text-justify}
* 1 écran LCD 16*02 avec backpack I2C
{: .text-justify}
* 4 fils souples connecteurs dupont mâle-mâle pour relier le PCB à l'écran LCD.
{: .text-justify}
* 1 connecteur coudé 4 pin 2.54mm à souder
{: .text-justify}
* 1 capteur de température et humidité DHT22 avec 4 broches
{: .text-justify}
* 1 diode Schottky SR560
{: .text-justify}
* 1 résistance 10KΩ
{: .text-justify}
* 1 condensateur céramique 100nf
{: .text-justify}
* 1 prise jack DC 3 pin à souder
{: .text-justify}
* 1 alimentation 5v 2A avec prise Jack
{: .text-justify}
* 4 entretoises M2.5 15mm en nylon, femelle-mâle 
{: .text-justify}
* 4 entretoise M2.5 6mm en nylon, femelle-mâle
{: .text-justify}
* 1 boîtier à imprimer 3D, les STL sont à récupérer dans la section **/STL** du [Github du projet](https://github.com/papsdroidfr/MeteoConnect).
{: .text-justify}

### Électronique

![MeteoCOnnect](/assets/images/tutos/036ESP32meteo/PCB_assemblage.jpg){: .align-center}
La photo ci-dessus illustre les soudures à faire sur le PCB. Les instructions sont clairement sérigraphiées sur la carte. Ne vous trompez surtout pas de sens avec la PYPSTICK ESP32-C3 ainsi que la diode Schottky.
{: .text-justify}

### Assemblage

{% include gallery id="gallery1" caption="" %}
Le boîtier est composé d'une partie basse dans laquelle se loge le circuit rehaussé avec les entretoises M2.5 en nylon (mettre les petites de 6mm en dessous, elles se vissent sur les grandes de 15mm qui vont supporter l'écran LCD.). La partie haute se clipse autour du LCD correctement positionné sur les entretoises 15mm. Il faut couper au cutter à raz bord du PCB les vis mâles en nylon qui dépassent pour pouvoir fermer le boîtier.
{: .text-justify}
Deux pieds inclinés (à coller avec de la colle PVC) peuvent se mettre sous le boîtier afin de faciliter la lecture de l'écran lorsqu'il est posé sur une table.
{: .text-justify}

## Software

### scripts micropython

Tous les scripts sont à récupérer dans la section /micropython du [Github du projet](https://github.com/papsdroidfr/MeteoConnect)

* [Cet article](https://papsdroidfr.github.io/dev/ESP32-LCD/) explique le fonctionnement d'un écran LCD avec un ESP32-C3
{: .text-justify}
* [Cet article](https://papsdroidfr.github.io/dev/ESP32-DHT22/) explique comment utiliser un DHT22 avec un ESP32-C3
{: .text-justify}
* [Cet article](https://papsdroidfr.github.io/dev/meteoESP32/) détaille l'utilisation d'une API de météo avec l'ESP32-C3 afin de récupérer par WIFI les données météorologiques extérieures.
{: .text-justify}

N'oubliez pas de **mettre à jour quelques paramètres** dans les scripts ci-dessous, en remplaçant les valeurs 'xxxxx':
{: .text-justify}

* **boot.py**: indiquez votre WIFI_SSID et WIFI_PASSWORD pour que le boîtier se connecte à internet via votre WIFI
{: .text-justify}
* **api_meteo.py**: indiquez votre TOKEN de connection à l'API comme expliqué dans [cet article](https://papsdroidfr.github.io/dev/meteoESP32/).
{: .text-justify}

### Calibration du capteur

## Usage
{% include gallery id="gallery2" caption="" %}