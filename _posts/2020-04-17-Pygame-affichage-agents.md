---
#source: https://www.papsdroid.fr/post/jeux-pygame-partie03
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
  teaser : "/assets/images/tutos/035Pygame/pygame03.png"

#SEO tags
title       : "Coder un jeux avec Pygame - 03 affichage des agents"
image       : "/assets/images/tutos/035Pygame/pygame03.png"
description : "Dans cette troisième partie du tutoriel pour apprendre à coder un jeux avec Pygame, nous allons voir comment gérer l'affichage de notre agent qui est un tank."

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Pygame"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "DEV" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["python3", "Pygame" ]

gallery1:
  - image_path: "/assets/images/tutos/035Pygame/pygame04.png"
  - image_path: "/assets/images/tutos/035Pygame/pygame05.png"
---

![Pygame](/assets/images/tutos/035Pygame/pygame03.png){: .align-left}
Dans cette troisième partie du tutoriel pour apprendre à coder un jeux avec Pygame, nous allons voir comment gérer l'affichage de notre "agent" qui est un tank composé de plusieurs images en incrustation, se déplaçant en vue du dessus sur un décors. Lors de ses déplacements nous allons devoir gérer **la rotation** des éléments qui composent notre agent, et de manière optimisée pour soulager un max les processeurs.
{: .text-justify}

## Prérequis
premières parties du tutoriel [dans ce billet](https://papsdroidfr.github.io/dev/Pygame-bases/)

## Rotation d'images, comment faire?
### Bibliothèque PIL.Image
La bibliothèque de gestion d'image **PIL.Image** dispose d'une méthode de rotation d'image que nous allons utiliser:
{: .text-justify}
```python
from PIL import Image

img_file = 'PATH/img.png'  #fichier image source
img= Image.open(img_file)
angle = 15
img_rotated = img.rotate(angle)
```
![Pygame](/assets/images/tutos/035Pygame/tank1.png){: .align-left}
![Pygame](/assets/images/tutos/035Pygame/tank2.png){: .align-rigth}

Notons que l'angle de rotation s’exprime en degrés (quart de tour = 90°, demi-tour = 180°, un tour complet = 360°), et non pas en radian (quart de tour = pi/2, demi-tour = pi, tour complet = 2*pi ...) et que l'image "img_rotated" a subie une rotation dans le sens inverse des aiguilles d'une montre.
{: .text-justify}

### Attention aux centres de rotation
Toute rotation implique un **centre de rotation** autour duquel tous les pixels de l'image subissent une rotation. Il y a deux pièges à éviter:
{: .text-justify}
* Certains pixels peuvent se retrouver après la rotation "en dehors" de la zone de définition de l'image originale, et dans ce cas vous allez vous retrouver avec une image cible "tronquée" sur les bords. Je conseille donc d'inclure les images dans **une zone de transparence 2 fois plus grande** pour éviter ce phénomène. Les images tanks sont en définition 64*64 pixels: je les ais recopiées dans une zone de transparence 128x128 pixels (avec GIMP).
{: .text-justify}
* Il faut s'assurer que **le centre de l'image corresponde bien au centre de rotation souhaité**. Par exemple dans le cas de notre tank, le centre de rotation doit correspondre au centre de la tourelle sur laquelle va se loger le canon. Il m'a fallu pour cela décaler l'image du tank vers le haut, sinon le centre de l'image ne correspondait pas tout à fait au centre de la tourelle (première image), et dans ce cas quand le tank subit des rotations il faut faire des calculs pour repositionner le canon au bon endroit .... Pour localiser le centre de l'image je créé un calque transparent sous GIMP avec deux diagonales rouges, puis je copie/colle mon tank dans un nouveau calque, ce qui me permet de déplacer le tank au bon endroit , et ensuite je supprime mes calques et écrase l'image d'origine: tout se fait facilement avec GIMP.
{: .text-justify}

{% include gallery id="gallery1" caption="rotation des agents" %}

Pour le canon, l'image qu'on récupère est centrée au milieu du canon... il faut décaler le canon pour que le début du canon soit positionné au centre de l'image, et comme ça il va pouvoir tourner parfaitement centré sur la tourelle du tank.
{: .text-justify}
>Mais si on souhaite malgré tout positionner un élément qui n'est volontairement pas centré au centre de l'image, comme par exemple une petite mitraillette devant le tank ?
{: .text-justify}

Dans ce cas il faut identifier **le décalage** par rapport au centre en nombre de pixels (dx,dy) et repositionner correctement l'image après rotation grâce à nos sympathiques amis sinus et cosinus. Après une rotation d'un angle "alpha", il faut alors repositionner l'image décentrée en position **(dx.cos(alpha) , dy.sin(alpha))**. Quand les images ont toutes le même centre de rotation au milieu de l'image: c'est plus simple on évite les sinus et cosinus.
{: .text-justify}
N'oublions pas non plus que si le corps du tank subit une rotation d'un certain angle, il faut propager cette rotation au canon aussi! sinon il resterait parfaitement immobile à l'écran tandis que le tank tourne ... Nous allons gérer tout cela de manière logicielle.
{: .text-justify}
> **Récapitulons: 1)** j'intègre mes images dans une zone de transparence deux fois plus grande. **2)** je vérifie que mon centre de rotation souhaité est bien positionné au centre de l'image. 3) une rotation de l'agent doit être propagée à toutes les images qui lui sont rattachées.
{: .text-justify}

## Pré-calcul des rotations
Afin d'éviter d'avoir à calculer des rotations d'images en temps réel pendant le jeux, même si ces calculs sont très rapides, nous allons définir un nombre de rotations nb_rotates et générer des listes de rotations de chaque images par pas de 360/nb_rotates. pour afficher l'agent dans la bonne rotation, il suffit alors de consulter dans les listes d'images lesquelles correspondent au rang de rotation voulu, et le tour est joué. Plus nb_rotates est grand, et plus le pas de rotation sera fin donnant l'illusion d'une rotation parfaitement fluide à l'écran.
{: .text-justify}

## Explications sur le code
Les sources sont à récupérer sur [ma page Github](https://github.com/papsdroidfr/Tuto_Pygame_Tank). Il s'agit du répertoire **\Tank03**, et ne pas oublier de récupérer tous les sous-dossiers où sont enregistrés les médias. Cette fois nous allons organiser les programmes pythons en créant un fichier par classe. Dans le programme principal **Tank03.py** on retrouve donc les imports des deux programmes class_Terrain.py et class_Tank.py dans lesquels nous décrivons nos classes (les extensions .py sont implicites dans les imports)
{: .text-justify}
```python
import pygame                     # PYGAME package
from pygame.locals import *       # PYGAME constant & functions
from sys import exit              # exit script
from PIL import Image             # bilbiothèque pour traitement d'image
from class_Terrain import Terrain # classe Terrain()
from class_Tank import Tank       # classe Tank() 
```

### La classe Terrain()
Tout est expliqué dans le [tutoriel 02](https://papsdroidfr.github.io/dev/Pygame-decors/).

### La classe Tank()
* **Le constructeur __init__()** de la classe va générer toutes les nb_rotates rotations de chaque image fournie dans la liste l_img_name. Il créé ainsi une nouvelle liste l_img_rotated dont chaque élément est une liste de nb_rotates images ayant subie une rotation par pas de 360/nb_rotates degrés. Ainsi l'élément l_img_rotated[n][r] représente l'image l_img_name[n] ayant subie une rotation de r*360/nb_rotates degrés dans le sens inverse des aiguilles d'une montre. Les rotations en cours de chaque image sont stockées dans la liste l_rotation.
{: .text-justify}
```python
self.l_img_rotated = self.rotates_l_img(l_img_name)
```
* **La méthode bouge()** va positionner et orienter le tank dont le paramètre self.human=True selon la souris. Un déplacement horizontal oriente le corps du tank, tandis qu'un déplacement horizontal oriente le canon. Bien entendu ce n'est que pour illustrer les rotations du tank et du canon, et s'assurer que tout est bien fluide et sans problème de centre de rotation mal placés. Le jeux final ne va pas se contrôler ainsi à la souris...
{: .text-justify}
* **La méthode dessine()** ne fait que "bliter" chaque image qui compose le tank, en allant rechercher dans la liste l_img_rotated l'image qui correspond à l'angle de rotation de l'image à afficher. Pour chaque image l_img_name[n] , son orientation est l_rotation[n]: il s'agit donc de bliter l'image   self.l_img_rotated[n][self.l_rotation[n]] et c'est là qu'on évite en temps réèl de calculer quelles rotations à appliquer à nos images.
{: .text-justify}
* **La méthode rotates_l_img()** va générer et retourner la liste l_img_rotated à partir de la liste d'images l_img_name et du nombre de rotations nb_rotates. Pour cela on y défini une fonction interne rotates_img() qui va générer et retourner une liste de nb_rotates images à partir d'une seule image img_file. Cette fonction fait aussi la conversion au passage d'une image au format PIL en image au format Pygame.
{: .text-justify}

### Ressources consommées sur un Raspberry pi
Au repos, mon Raspberry pi4 (4Go) affiche une occupation des 4CPUs à 0.5% et RAM utilisée 271 Mo (mesures prises avec l'utilitaire top). Lorsque j'exécute le programme Tank03.py l'occupation des 4 CPUs passe à 22,5% et la RAM utilisée: 316Mo. Le programme consomme donc **22% de CPUs** et **45Mo de RAM** uniquement: ça passera sans pb sur un pi3 aussi, et il reste donc suffisamment de CPUs pour gérer le gameplay du jeux.
{: .text-justify}
>Prochain tuto: gérer les déplacements avec les décors, notamment les collisions avec les zones interdites pour éviter que notre tank ne traverse les murs ou un autre tank par exemple.
{: .text-justify}