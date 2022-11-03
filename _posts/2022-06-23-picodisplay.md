---
#source: https://www.papsdroid.fr/post/pico-display
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
  teaser : "/assets/images/tutos/031PicoDisplay/picodisplay-300.jpg"

#SEO tags
title       : "Affichage PicoDisplay  Pimoroni"
image       : "/assets/images/tutos/031PicoDisplay/picodisplay-300.jpg"
description : "Configuration d'un picoDisplay de Pimoroni sur un Raspberry Pico"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "PicoDisplay"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pico" , "micro-python", "Pimoroni" ]

---
![PicoDisplay](/assets/images/tutos/031PicoDisplay/picodisplay-300.jpg){: .align-left}
J'ai acheté un [pico display](https://shop.pimoroni.com/products/pico-display-pack?variant=32368664215635) de [Pimoroni](https://shop.pimoroni.com/). il s'agit d'une petite dalle couleur 1.14" IPS LCD 240*135 pixels avec 4 petits boutons poussoirs et une led RGB à disposer sur un Raspberry PICO (muni de ses pin header mâle soudès). Mon projet en cours consiste à créer un petit compte à rebours de cuisine en utilisant ce matériel. Cette article détaille comment configurer le Raspberry pico avec un pico display.
{: .text-justify}

## Assemblage hardware
Tout est très bien expliqué sur [le site Pimoroni](https://learn.pimoroni.com/article/getting-started-with-pico) avec photo montage qui expliquent comment souder correctement (et dans le bon sens ...) les pin header sur le Raspberry pico pour ensuite assembler le pico display. Le logo "pico display" ainsi que les boutons A et B doivent être orientés vers la sortie USB du Rapberry PICO.
{: .text-justify}

## configuration software
Pour ce qui est de la configuration software, c'est le b..... à l'heure où j’écris cet article, à cause des changements majeurs en cours que Pimoroni propose dans la bibliothèque micropython qui gère le driver et l'absence de documentation. Il ne faut pas faire les flashages par défaut ni tester les programmes mis en exemple: rien ne fonctionne, et pour couronner le tout, la documentation Github officielle en lien sur leur page renvoie sur une erreur 404 "page not found"! En passant par les forums, j'ai fini par comprendre ce qu'il faut faire, j'en détaille ici les grandes lignes, et ça en vaut la peine car cette petite carte est vraiment fabuleuse.
{: .text-justify}

### flashage drivers micropython
Si vous débutez avec les Raspberry PICO je vous encourage à lire [cet article](https://papsdroidfr.github.io/configuration/pico/).
{: .text-justify}
La première étape consiste à flasher le bon ficher uf2 sur le Raspberry PICO. Il faut prendre la version 1.19.0 à minima que propose Pimoroni: les version précédentes ne fonctionneront pas avec l'exemple de code fourni plus bas.
{: .text-justify}
* récupérez le fichier **uf2 (version 1.19.0 minimale)** sur le [Github Pimoroni](https://github.com/pimoroni/pimoroni-pico/releases). dans la section Asset (vous avez une liste de fichiers, téléchargez le [pimoroni-pico-v1.19.0-micropython.uf2](https://github.com/pimoroni/pimoroni-pico/releases/download/v1.19.0/pimoroni-picolipo_4mb-v1.19.0-micropython.uf2)) 
{: .text-justify}
* pour flasher ce fichier sur le Raspberry PICO: maintenez enfoncé le bouton poussoir du pico en le branchant à l'ordinateur (via l'USB): cela ouvre un répertoire sur lequel il suffit de copier et coller le fichier uf2. Le pico reboot tout seul, il est prêt.
{: .text-justify}

Dans la documentation officielle de Pimoroni ils continuent de faire référence à la version officielle 1.18 dont toute la documentation a disparue et je peux vous confirmer qu'aucun de leurs exemples test ne fonctionne: les bibliothèques ne sont pas reconnues.
{: .text-justify}

### test micropython
Avec [Thonny](https://thonny.org/) vous pouvez tester ce petit programme simple qui permet d'afficher un texte à l'écran, de contrôler la led RGB et d'avoir une interaction avec le bouton poussoir A: ça donne les bases pour exploiter cette belle petite carte.
{: .text-justify}

La documentation du driver est en cours d'écriture par Pimoroni sur [ce Github](https://github.com/pimoroni/pimoroni-pico/tree/main/micropython/modules/picographics). Cette doc explique bien comment interagir avec l'écran pour écrire du texte ou faire des dessins, afficher une image etc mais rien au sujet du contrôle de la led RGB ni des boutons poussoirs: j'ai fini par trouver en allant voir d'autres exemples sur les forums.
{: .text-justify}

>Je reprécise que ce code ne fonctionne qu'avec à minima la version **uf2 1.19.0** produite par Pimoroni.
{: .text-justify}

```python
import utime
from picographics import PicoGraphics, DISPLAY_PICO_DISPLAY, PEN_RGB332
from pimoroni import Button
from pimoroni import RGBLED

print('test pico display')

button_a = Button(12)
button_b = Button(13)
button_x = Button(14)
button_y = Button(15)

led = RGBLED(6, 7, 8)


display = PicoGraphics(display=DISPLAY_PICO_DISPLAY, rotate=0, pen_type=PEN_RGB332 )

display.set_backlight(0.5)

display.set_pen(display.create_pen(255, 0, 0)) # Set a red pen
display.clear()  # Clear the display buffer
display.set_pen(display.create_pen(255, 255, 255))   # Set a white pen
display.text("pico display", 10, 10, 240, 6)  # Add some text
display.update()  # Update the display with our changes

led.set_rgb(255, 0, 0)   # Set the RGB LED to red
utime.sleep(1)           # Wait for a second
led.set_rgb(0, 255, 0)   # Set the RGB LED to green
utime.sleep(1)           # Wait for a second
led.set_rgb(0, 0, 255)   # Set the RGB LED to blue

while button_a.read() == False:
    pass

led.set_rgb(0, 255, 0)  # Set the RGB LED to green
```

Vous devriez voir s'afficher le texte "pico display" à l'écran sur fond rouge tandis que la LED RGB clignote en rouge, vert, bleu. Si vous appuyez sur le bouton poussoir A, la led redevient verte.
{: .text-justify}