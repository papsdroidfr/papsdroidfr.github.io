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
Ce projet détaille pas à pas la construction d'une petite station météo de salon connectée, à base d'un [ESP32-C3](https://papsdroidfr.github.io/configuration/Pybstick-C3/) made in France. Un capteur de température interne DHT22, qui sera calibré par le logiciel, permet d'afficher la température et l'humidité de la pièce dans laquelle il fonctionne, tandis que les données météorologiques externes sont collectées sur internet en WIFI par un appel d'API. Le tout est affiché sur un petit écran LCD composé de 2 lignes, 16 caractères. L'électronique est logée dans un petit boîtier sur mesure, à imprimer en 3D.
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

![MeteoCOnnect](/assets/images/tutos/036ESP32meteo/qualibrate.jpg){: .align-left}
Le DHT22 est un capteur sensible, il suffit de s'en approcher pour voir immédiatement quelques dixièmes de degrés supplémentaires apparaître. Comme il est soudé sur la carte, l'électronique à proximité va perturber les mesures bien que l'ESP32-C3 soit sobre en consommation: rien ne chauffe. Malgré tout, une petite dérive s'installe et il faut procéder à une calibration pour compenser cette dérive.
{: .text-justify}

#### Principe

Il faudra vous équiper d'un thermomètre de référence, de préférence un tout simple non électronique au mercure. Le principe est de prendre **2 mesures assez éloignées**  avec 4 ou 5°C d'écart (une dans la pièce courante, et une à
l'extérieur). Ensuite la calibration consiste à faire une interpolation linéaire à partir de ces 2 mesures.
{: .text-justify}

#### Procédure

* Il faut tout d'abord **régler sur Off la calibration** (elle est sur On par défaut avec mes propres mesures): dans le programme dht22.py mettez la variable en ligne 6  **QUALIBRATION = False** (à la place de QUALIBRATION=True)
{: .text-justify}
* Ensuite prenez vos deux mesures éloignées de quelques degrès, attendez bien à chaque fois un bon 1/4 d'h pour que les mesures soient stabilisées. Notez-les (T_DHT1, T_R1) et (T_DHT2, T_R2), T_DHTx étant la température indiquée par le DHT22, et T_Rx étant celle indiquée par votre thermomètre de référence.
{: .text-justify}
* Reportez les valeurs dans le programme dht22.py, lignes 7 à 10, et remettez la variable QUALIBRATION = True en ligne 6
{: .text-justify}

Le programme va alors calibrer la température T mesurée par le DHT22 selon cette formule:
{: .text-justify}

```maths
Tc  =  r * T + T0
r = (T_R2 - T_R1) / (T_DHT2 - T_DHT1)
T0 = TR2 - r * T_DHT2
```

> Et voilà la calibration est terminée: la température affichée par le DHT22 devrait être équivalente à celle du thermomètre de référence dans vote pièce principale.
{: .text-justify}

## Usage
{% include gallery id="gallery2" caption="" %}
La première ligne donne les valeurs du capteur interne DHT22: température et humidité. Ces valeurs sont mises à jour toutes les 5 secondes.
{: .text-justify}

La seconde ligne affiche les données météorologiques externes récupérées sur internet et mises à jour toutes les 5 minutes.
{: .text-justify}

* dans un premier temps on affiche la température externe et le taux d'humidité
{: .text-justify}
* ensuite il s'affiche la vitesse du vent et la probabilité qu'il pleuve
{: .text-justify}
* Enfin le bulletin météorologique est scrollé à l'écran.
{: .text-justify}