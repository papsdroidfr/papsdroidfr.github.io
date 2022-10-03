---
#source: https://www.papsdroid.fr/post/sysdroidoled
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
  teaser : "/assets/images/tutos/003sysdroid/04sysdroidOLED/sysdroid-300.jpg"

#SEO tags
title       : "Sysdroid: monitoring Raspberry pi sur écran OLED"
image       : "/assets/images/tutos/003sysdroid/04sysdroidOLED/sysdroid-300.jpg"
description : "monitoring CPU, RAM Disque Raspberry pi sur un écran OLED"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Sysdroid"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pi" ,  "python3", "électronique", "monitoring", "écran OLED" ]

---

![SysdroidOLED](/assets/images/tutos/003sysdroid/04sysdroidOLED/sysdroid-300.jpg){: .align-left}
Ce projet complète ma collection de **monitoring temps réel** des CPUs, RAM, espace de stockage et température système d'un Raspberry pi sur un petit **écran OLED** I2C 128*64 pixels. Il fonctionne avec un pi3 ou un pi4 et ne nécessite aucune électronique, donc pas de carte PCB à commander, ni de soudure à prévoir.
{: .text-justify}
## Matériel nécessaire
* Un raspberry pi 3 ou 4. Dans mon cas j'ai un pi4 qui démarre sur un SSD M2 dans un boîtier ArgonOne M.2 en alliage d’aluminium, ce qui lui permet de fonctionner en fanless à des températures inférieures à 55°C. Si vous souhaitez configurer votre pi4 ainsi, suivez [ce guide](https://papsdroidfr.github.io/configuration/ArgonOneM2/).
{: .text-justify}
* Un petit écran OLED monochrome I2C (il n'a que 4 broches) 128*64 pixels. J'ai pris des modèles AZDelivery qui sont vraiment pas cher du tout (5 pour une poignées d'€)
{: .text-justify}
* une nappe de 4 câbles dupond souples femelle/femelle (je les achète par paquets de 20)
{: .text-justify}
* optionnel: un cache à imprimer 3D. Le STL est disponible dans le dossier STL3D du [Github du projet](https://github.com/papsdroidfr/Sysdroid_OLED).
{: .text-justify}

## Assemblage
![SysdroidOLED](/assets/images/tutos/003sysdroid/04sysdroidOLED/fritzing.png){: .align-left}
Il suffit de relier les 4 fils sur les bonnes broches GND, VCC(3.3v), SDA et SCL entre le raspberry pi et l'écran OLED. Attention la moindre erreur avec GND ou VCC, et l'écran OLED ne survit pas .... Si vous utilisez un boîtier en alliage d’aluminium qui évacue la chaleur du raspberry : il chauffe. Il faut donc veiller à disposer la nappe en dessous du boîtier et non pas au dessus.
{: .text-justify}
**Petite vidéo d'assemblage expliquée par noter mascotte Babbage bear lui-même!**
{% include video id="dLdJ5HpMTOs" provider="youtube" %}

Attention dans mon cas j'utilise un boîtier **ArgonOne M.2** avec le connecteur GPIO 20 broches déporté et surtout inversé par rapport au connecteur du Raspberry. Si vous branchez directement sur les broches GPIO du Raspberry ne vous fiez pas à ma vidéo: fiez-vous au brochage du Raspberry pour repérer GND, VCC(3.3v), SDA et SCL qui devraient se trouver plutôt à gauche qu'à droite.
{: .text-justify}

## Préparation du système
### dépendance bibliothèques
Tous les scripts python ci-dessous se trouvent dans le dossier python du [Github du projet](https://github.com/papsdroidfr/Sysdroid_OLED).
{: .text-justify}
* l'écran OLED I2C sera piloté par la bibliothèque d'Adafruit. Elle nécessaire préalablement d'installer [circuitPython](https://learn.adafruit.com/welcome-to-circuitpython/what-is-circuitpython) sur le Raspbery pi (adafruit_blinka). Exécuter le script blinka_test.py avant d'aller plus loin pour être certain d'avoir bien installé la bibliothèque.
{: .text-justify}
* Installer ensuite la bibliothèque [adafruit_circuitpython_ssd1306](https://learn.adafruit.com/monochrome-oled-breakouts/python-setup) ainsi que la PILLOW library.
{: .text-justify}
* Activez le bus I2c du raspberry pi (via sudo raspi-config -> 3 Interface Options -> P5 I2C -> OUI). Un reboot sera nécessaire. Il n'est pas nécessaire de speeder le bus I2C pour ce projet.
{: .text-justify}
* Exécuter le script **oled_I2C_test.py** pour vous assurez que tout fonctionne.
{: .text-justify}

### script principal
Déposez dans **/home/pi** le script **sysdroid_oled.py**. Vous pouvez l'exécuter pour vérifier que le monitoring des CPUs, RAM, Disk et température démarre.
{: .text-justify}
![SysdroidOLED](/assets/images/tutos/003sysdroid/04sysdroidOLED/chrome_web.jpg){: .align-left}
* Sur la gauche s'affiche l'état des 4 processeurs sous forme de barres de niveaux verticales
{: .text-justify}
* En haut à droite: il y a le niveau de la RAM puis le niveau de l'espace Disk sous forme de barre de niveaux horizontales
{: .text-justify}
* en bas à droite est indiquée la température des processeurs en °C. Les données sont mises à jour toutes les 2 secondes.
{: .text-justify}
> Ce script consomme moins de 1% des capacités du raspberry pi.
{: .text-justify}

### automatisation au démarrage du pi
Pour que le monitoring démarre automatiquement avec le démarrage du raspberry pi, il faut ajouter une commande dans la cron table:
{: .text-justify}
```bash
sudo nano /etc/crontab -e
```
ajouter cette ligne à la fin:
```
@reboot pi python3 'sysdroid_oled.py' &
```
ctrl-O pour enregistrer, puis ctrl-X pour quitter.
{: .text-justify}
Le monitoring va démarrer à chaque reboot du Raspberry.
{: .text-justify}

## cache imprimé 3D
![SysdroidOLED](/assets/images/tutos/003sysdroid/04sysdroidOLED/cache3D.png){: .align-left}
Un petit cache optionnel peut être imprimé 3D (18mn en 0.2mm, remplissage 17%). Le STL est à récupérer dans le dossier **/STL3D** du [Github](https://github.com/papsdroidfr/Sysdroid_OLED). Je l'ai imprimé avec avec du noir Chromatik.
{: .text-justify}

![SysdroidOLED](/assets/images/tutos/003sysdroid/04sysdroidOLED/sysdroid.jpg){: .align-center}