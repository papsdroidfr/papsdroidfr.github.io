---
#source: https://www.papsdroid.fr/post/jeux-pygame-partie02
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
  teaser : "/assets/images/tutos/035Pygame/pygame02-300.jpg"

#SEO tags
title       : "Coder un jeux avec Pygame - 02 les décors"
image       : "/assets/images/tutos/035Pygame/pygame02-300.jpg"
description : "Dans cette seconde partie nous allons voir comment générer un décors pour notre jeux à partir de tuiles qui sont des petites images de 64*64 pixels avec un canal alpha de transparence pour permettre les incrustations de plusieurs images les unes sur les autres."

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Pygame"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "DEV" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["python3", "Pygame" ]

---
![Pygame](/assets/images/tutos/035Pygame/pygame02-300.jpg){: .align-left}
Dans cette seconde partie nous allons voir comment générer un décors pour notre jeux à partir de "tuiles" qui sont des petites images de 64 * 64 pixels avec un canal alpha de transparence pour permettre les incrustations de plusieurs images les unes sur les autres. L'idée générale va être de décrire cette structure dans des fichiers texte qui seront lus par le programme Python pour construire l'image du Background.
{: .text-justify}

## Pré-requis:
* Ce billet est la seconde partie d'un tutoriel qui démarre [ici](https://www.papsdroid.fr/post/jeux-pygame).
{: .text-justify}
* Le programme python (orienté objet) nécessite de bien comprendre l'usage des **dictionnaires** et des **listes**. Si ces mots ne vous disent rien: faites un petit tour sur les moteurs de recherche pour bien comprendre ces notions ;-) qui sont très puissantes et indispensables à tout programmeur Python.
{: .text-justify}
* Télécharger le code et les médias sur [ma page Github](https://github.com/papsdroidfr/Tuto_Pygame_Tank). Il s'agit du dossier **/TANK02** à cloner dans son intégralité sans oublier les sous-dossiers /Media et /Map
{: .text-justify}

## Comment créer un background à partir d'images 64 * 64 pixels ?
On va définir dans un premier temps le nombre de tuiles que nous voulons utiliser, par exemple 12 lignes de 18 tuiles chacune faisant 64 * 64 pixels, cela donne une image de 768 * 1152 pixels. Cette image est donc composée de 216 tuiles réparties sur une matrice de 12 * 18.
{: .text-justify}
Pour constituer une image, il me suffit de coder chaque tuile avec une codification arbitraire A1, A2, A3 etc .... et d'écrire cette suite de codes dans un simple fichier texte: ce sera notre fichier "map". Le programme python va devoir lire ce fichier "map" et aller rechercher l'image correspondante (c'est là qu'un dictionnaire va nous être très utile ...)
{: .text-justify}
Chaque tuile étant une image .png avec un code alpha qui gère la transparence: je peux donc superposer à l'écran plusieurs "map" les unes sur les autres, ce qui me permet de construire mon décors en plusieurs étapes: le fond d'abord, puis les murs/fondations, enfin quelques végétaux et roches et pour finir le bord de l'écran.
{: .text-justify}
**Donc pour résumer, nous allons:**
{: .text-justify}
* Associer chaque image 64 * 64 pixels à un code unique, à l'aide d'un dictionnaire.
{: .text-justify}
* Décrire nos "map" dans des fichiers textes où l'on doit retrouver les codes correspondants aux images.
{: .text-justify}
* Définir l'ordre des maps à lire dans une liste de maps.
{: .text-justify}
* Ecrire un lecteur de fichier map qui va transformer une map en une matrice de 12 * 18 images (dans l'hypothèse où les map sont décrites en 12 * 18).
{: .text-justify}
* Constituer une image background en fusionnant l'une après l'autre chaque matrice d'image avec la suivante selon l'ordre des maps établi.
{: .text-justify}
>Et voilà, comme ça nous avons notre image background définitive, et même en empilant presque une dizaine de map l'une sur l'autre en 12 * 18 ça se passe très très vite sur un Raspberry pi!
{: .text-justify}

## Organisons le travail de manière "orientée objet"
L'idée est de décrire les objet (définis sous formes de Classes) dont nous avons besoin et qui regroupent leurs propres variables et méthodes. On a besoin:
{: .text-justify}
* **d'une Classe Terrain** qui va initialiser (via son constructeur) le dictionnaire des codes pour les images 64 * 64, définir la taille du terrain de jeux (12 * 18 par exemple), définir la liste des maps à charger, et construire le background final en lisant la liste de maps à lui fournir. Cette classe sera dotée d'une méthode de lecture de map , et d'une méthode de dessin qui va permettre d'afficher à l'écran le background construit.
{: .text-justify}
* **d'une Classe Tank**: cette classe va initialiser (via son constructeur) l'image du tank en combinant plusieurs images (notamment le corps du tank et son canon: si on veut les articuler plus tard il nous faudra bien 2 images distinctes), et initialiser son comportement pour savoir s'il est contrôlé par un humain ou par la machine. Elle sera dotée d'une méthode de dessin pour afficher le tank à l'écran. Rien de plus pour le moment, toute l'animation des "agents" fera l'objet d'un autre tuto chaque chose en son temps. Pour l'instant le tank "contrôlé par un humain" sera positionné à l'écran là où se situe la souris grâce à sa méthode bouge().
{: .text-justify}
Maintenant si vous regardez de plus près le code python téléchargé depuis ma page Github vous allez bien comprendre la logique.
{: .text-justify}

## Quelques explications sur le code
### Bibliothèques
Pour faire les traitements d'images (fusions des images 64 * 64) nous allons utiliser depuis la bibliothèque **PIL** (normalement déjà installée avec Python 3) le module Image
{: .text-justify}
```python
import pygame                   # PYGAME package
from pygame.locals import *     # PYGAME constant & functions
from sys import exit            # exit script
from PIL import Image           # bilbiothèque pour traitement d'image
```

### Classe Terrain()
Ensuite vient la définition de la **classe Terrain()**:
{: .text-justify}
#### Le constructeur
Le constructeur prend en variables la liste des fichiers textes décrivant les maps, puis les tailles des tuiles (sprites en anglais) et leur quantité (convention habituelles avec le traitement d'images: X=colonnes, Y=lignes)
{: .text-justify}
```python
class Terrain():
    def __init__(self, map_filenames, size_spriteX=64, 
        size_spriteY=64, nb_spritesX=18, nb_spritesY=12):
```
Rien de bien sorcier ensuite si vous avez suivi le 1er tuto: le constructeur initialise pygame, jusqu'à la définition du dictionnaire des images 64 * 64 self.sprites: on associe un code unique (key du dictionnaire) à chaque fichiers .png dont le chemin est initialisé dans la variable **self.path_media = 'Media/Decors/'**
{: .text-justify}
```python
self.sprites = {'  ': None,   
    '00': 'Ground_Tile_01_A_64x64.png', # fond désert 1
    '01': 'Ground_Tile_02_A_64x64.png', # fond désert 2
    'h1': 'Hedge_A_01_64x64.png',       # bord haut gauche
    'h2': 'Hedge_A_01b_64x64.png',      # bord haut droite
    'h3': 'Hedge_A_01c_64x64.png',      # bord bas droite
...
}
```
Ensuite on va construire **un second dictionnaire self.sprites_pil** en associant cette fois chaque code du dictionnaire précédent à une image lue par le module PIL.Image. A noter que dans le 1er dictionnaire on a un code ' ' pour définir l'absence d'image (on n'est pas obligé de mettre des image partout sur notre map ...): le second dictionnaire doit exclure ce code pour lire les images et le code est ajouté ensuite pour associer la clé ' ' à None (aucune image)
{: .text-justify}
```python
self.sprites_pil = { 
    key: Image.open(self.path_media+self.sprites[key]).convert('RGBA')
    for key in self.sprites.keys()
    if key != '  '
}
self.sprites_pil['  '] = None 
```
>Comprenez bien à ce stade l'utilité de ces 2 dictionnaires: le premier **self.sprites** regroupe les associations 'code':'nom_fichier_image.png' et le second dictionnaire **self.sprites_pil** va quand à lui regrouper les associations 'code':image qui sont toutes au format 'PIL'. Nous verrons plus tard qu'il faudra les convertir pour que Pygame puisse les lire.
{: .text-justify}
Ensuite nous appelons la méthode lire_map de notre objet pour construire une matrice matrix_map_pil des images (au format PIL) correspondant au 1er fichier fournis dans la liste map_filenames. J'expliquerais plus bas le principe de cette méthode. Mais retenez que le résultat est une liste python d'images 64*64 pixels au format PIL (XiYj représentant la colonne i sur la ligne i):
{: .text-justify}
```python
 [     [img_x1y1,  img_x2y1,   ...,  img_x18y1 ],

       [img_x1y2,  img_x2y2,   ...,  img_x18y2 ],

       ...

       [img_x1y12, img_x2y12, ..., img_x18y12 ]

]
```

par exemple matrix_map_pil[8] représente la liste des 18 images de la 9ème ligne (la première étant matrix_map_pil[0]). Si on veut atteindre la 5ème image de cette 9ème ligne, on l'obtient avec matrix_map_pil[8][4]. Le premier index c'est pour les lignes, et le second pour les colonnes.
{: .text-justify}
```python
matrix_map_pil = self.lire_map(map_filenames[0])
```
Une fois qu'on a pu charger toutes les images de la première map dans une matrices d'images, on va lire les suivantes (d'où le map_filenames[1:]) et les fusionner l'une après l'autre avec la précédente en utilisant la méthode Image.alpha_composite de la bibliothèque PIL. si on tombe sur un code None dans notre matrice évidemment on ne fusionne rien. Image.alpha_composite prend 2 arguments: le premier c'est l'image source sur laquelle on fusionne la seconde image dans le second argument.
{: .text-justify}
```python
for map_filename in map_filenames[1:]: 
   matrix_map_add_pil = self.lire_map(map_filename) 
       for y in range(self.nb_spritesY):
           for x in range(self.nb_spritesX):
               if matrix_map_add_pil[y][x] != None : # image non vide
                   matrix_map_pil[y][x] = 
                      Image.alpha_composite(matrix_map_pil[y][x],
                                            matrix_map_add_pil[y][x])  

```
>A ce stade nous avons modifié toutes les images 64*64 pixels dans notre matrice matrix_map_pil en fusionnant les images de la liste des map_filenames fourni au constructeur.
{: .text-justify}
Nous allons ensuite construire notre image cible au format du nombre de pixels total calculé selon le nombre de tuiles et leur taille, puis déposer à leur place chacune des images 64 * 64 de la matrix_map_pil. On utilise pour cela la fonction .paste() d'un objet PIL qui prend comme arguments l'image à coller et la position exprimée sous forme de tuple (x,y) représentant les coordonnées du coin supérieur gauche.
{: .text-justify}
```python
background_pil = Image.new('RGBA',(self.nb_pixelsX, self.nb_pixelsY), 0) 
for y in range(self.nb_spritesY):
    for x in  range(self.nb_spritesX):
        background_pil.paste(
                        matrix_map_pil[y][x], 
                        (x*self.size_spriteX, y*self.size_spriteY)
                      )
```
C'est presque fini ! pour que cette image background_pil puisse être lue par Pygame il va falloir la convertir, ce qui ce fait avec cette ligne de code (les images étant toutes au format "RGBA". En gros on convertie les images PIL avec la méthode .tobytes() et Pygame peut reconstruire on image avec la méthode pygame.image.fromstring()
{: .text-justify}
```python
self.background_img = pygame.image.fromstring(
    background_pil.tobytes(), 
    background_pil.size, 
    'RGBA'
    )
```
>Ça y est, notre constructeur a fabriqué le décors self.background_img que Pygam va pouvoir afficher maintenant.
{: .text-justify}

#### La méthode dessine()
Alors là rien de plus simple, il suffit de "blitter" l'image self.background_img en position (0,0), et  rien de plus !
{: .text-justify}
```python
self.screen.blit(self.background_img, (0,0))
```
Je me suis quand même posé la question de savoir ce qui est plus efficace entre fabriquer une grosse image qui occupe tout l'écran que l'on "blit" d'un seul coup, et "bliter" les 12*18 images (216) chacune à leur place. Si vous avez lu le premier tuto vous savez que le corps de la boucle infinie Pygame va constamment rafraîchir l'écran avec ce qu'on a "blité" en mémoire. Quand je "blit" les 216 petites images de 64*64 chacune à leur place (ce qui me contraint d'abord à toutes les convertir en images Pygame) j'ai un des 4 processeurs du raspberry pi4 qui est **occupé à 75%**. Quand je "blit" la grosse image d'un coup: ce processeur tombe à **47%**. Normal en fait car bliter en temps réel 216 images même plus petites qu'une seule qui prend toute la zone ça mange des ressources.
{: .text-justify}
**MAIS** si dans dans notre cas on s'en sort bien comme ça ("bliter" une seule image d'un coup), dans un jeux où l'on voudra scroller l'écran lors du déplacement des agents (type zelda-like) il faudra en fait procéder à **découper l'écran en plusieurs zones** blitées une par une (pas 216 zones non plus: 16 zones (4*4) par exemple) ce qui permet de préparer en mémoire les zones qui risquent de s'afficher lorsque l'agent sort de l'écran, sans avoir à calculer un nouvel affichage par combinaison d'autres énormes images qui boufferait pour le coup beaucoup de ressources et ferait "lagger" le jeux à ce moment là.
{: .text-justify}

#### La méthode lire_map()
Elle prend en paramètre un nom de fichier "map" à lire, qui doit respecter ce principe:
{: .text-justify}
* les lignes qui commencent par # sont des commentaires à ignorer: ce permet par exemple d'y enregistrer le dictionnaire des codes, ce qui est pratique à l'usage ....
{: .text-justify}
* les lignes qui commencent par un M sont les lignes de codes du dictionnaire des images.
{: .text-justify}
* chaque code  est séparé par un caractère "pipe" (bare verticale) 
{: .text-justify}
* le code '  ' (double espace) est réservé absolument pour matérialiser l'absence d'une image à cet endroit.
{: .text-justify}
* Enfin on évite de faire des codes qui commencent par 'M' puisque c'est l'indicateur utilisé pour démarrer la lecture d'une ligne de codes.
{: .text-justify}

Je n'ai pas passé de temps à écrire un "interpréteur" qui gérerait toutes les erreurs de cohérence, par exemple s'il manque des "pipe" ou si le nombre de lignes et de colonnes ne correspond pas à la définition du jeux, ou encore si on utilise un code qui n'existe pas dans le dictionnaire etc ... A voir plus tard mais donc attention la moindre erreur comme ça dans ce fichier .map et le programme plante: ce sera a améliorer plus tard.
{: .text-justify}
Le code commence donc simplement par une **lecture de fichier texte** où l'on ignore les lignes qui commencent par #, on récupère des codes séparés par des "pipe" , et on ignore les codes qui commencent par M ou le retour chariot ('\n') récupéré en fin de ligne. Ces codes sont ajouté à notre liste en deux dimensions matrix_map où matrix_map[y][x] repésente donc le code de la ligne y (0 étant la 1ère) et colonne x
{: .text-justify}
```python
matrix_map = []
with open(map_file_name, 'r') as f: 
    for line in f:       
        if line[0] == 'M':       
            codes = line.split('|') 
                matrix_map.append(
                    [ c for c in codes 
                      if c!='\n' and  c[0]!='M'
                     ] ) 
```
Ensuite une fois qu'on a constitué notre liste de codes matrix_map, on construit  la liste correspondante d'images PIL en consultant le dictionnaire des images self.sprites_pil 
{: .text-justify}
(je vous l'avais dit qu'il fallait être à l'aise avec les listes et les dico python hein ? On y arrive en une seule ligne de code ...)
{: .text-justify}
```python
matrix_map_pil = [ [ self.sprites_pil[matrix_map[y][x] ]
   for x in range(self.nb_spritesX) ]
   for y in range(self.nb_spritesY)
                          ]  
```
Et voilà il ne reste qu'à retourner cette liste:
{: .text-justify}
```python
return matrix_map_pil
```
>**Ouf c'est terminé** avec cette classe Terrain() qui est donc capable de lire une série de map en fichiers texte, et de les convertir en une image backgound. J'ai ajouté des maps et des images dans le dossier TANK02 de Github et vous pouvez constater que la génération de l'image backgounrd en empilant 6 maps de 12*18 est extrêmement rapide.
{: .text-justify}
{% include figure image_path="/assets/images/tutos/035Pygame/pygame02.jpg" caption="Pas belle cette map générée par un Raspberry pi ?" %}

### La classe Tank()
Rien de bien complexe à ce stade car ça fera l'objet d'autres tutoriels, donc je ne vais pas trop m’étendre. Son constructeur initialise diverses variables et notamment les 2 images qu'il va falloir "blitter". Pour positionner les éléments au centre (le canon par exemple) il faut faire attention à prendre en compte la dimension des objets (taille du corps et taille du canon), et dans ce cas c'est pratique de pré-calculer une fois pour toute les 1/2 tailles pour éviter d'avoir à le faire tout le temps...
{: .text-justify}
la méthode **dessine()** du tank va blitter son corps et son canon centré sur le corps.
{: .text-justify}
La méthode **bouge()** va positionner le tank centré sur la souris pour les tank initialisés avec human=True dans le constructeur. C'est donc normal qu'à l'exécution du pgm vous ne voyez qu'un seul tank bouger avec votre souris, et l'autre reste figé sur sa base.
{: .text-justify}

### La classe Game()
C'est notre jeux principal qui va instancier dans son constructeur un objet Terrain en lui fournissant la liste des fichiers map à lire, puis initialiser une liste de 2 tanks dont 1 est contrôlé par un humain.
{: .text-justify}
Les deux méthode loop() et destroy() sont celles déjà expliquées dans le tutoriel01. Dans la loop on capte les évènements pour réagir au click "QUIT", et ensuite on dessine le terrain, puis dessine et fait bouger les tanks.
{: .text-justify}
>Prochain tutoriel à venir: la gestion de l'animation des tanks.
{: .text-justify}