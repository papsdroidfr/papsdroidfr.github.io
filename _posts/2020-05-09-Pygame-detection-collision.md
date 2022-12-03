---
#source: https://www.papsdroid.fr/post/jeux-pygame-partie05
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
  teaser : "/assets/images/tutos/035Pygame/pygame07.png"

#SEO tags
title       : "Coder un jeux avec Pygame - 05 détecter les collisions"
image       : "/assets/images/tutos/035Pygame/pygame07.png"
description : "Dans cette cinquième partie du tutoriel apprendre à coder un jeux avec Pygame, nous allons voir comment détecter les collisions entre nos agents et les décors de manière à pouvoir interagir avec les décors (comme interdire de traverser un mur)."

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Pygame"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "DEV" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["python3", "Pygame" ]

---

![Pygame](/assets/images/tutos/035Pygame/pygame07.png){: .align-left}
Dans cette cinquième partie du tutoriel [apprendre à coder un jeux avec Pygame](https://www.papsdroid.fr/post/jeux-pygame), nous allons voir comment détecter les collisions entre nos agents et les décors de manière à pouvoir interagir avec les décors (comme interdire de traverser un mur). Dans Pygame il y a une mécanique de détection de collisions basée sur des rectangles: nous allons donc "cribler" notre map et nos agents de rectangles (plus ils seront fins, et plus la détection de collision sera réaliste).
{: .text-justify}
## Pré-requis
* Si vous arrivez au hasard d'une recherche sur ce billet, il s'agit de la 5ème partie d'un tutoriel qui démarre [ici](https://www.papsdroid.fr/post/jeux-pygame).
{: .text-justify}
* Dans ce tutoriel je vais exploiter au maximum les notions de tuples, de listes et de dictionnaires Python: on va même créer des listes de dictionnaires! Si vous n'êtes pas à l'aise avec ces notions il est préférable de les approfondir avant (il y a plein de guides sur le web).
{: .text-justify}
* L'intégralité du code Python + médias (open source libre de droits) est disponible sur [ma page Github](https://github.com/papsdroidfr/Tuto_Pygame_Tank), il s'agit du dossier **\Tank05** à récupérer dans son intégralité avec tous les sous-dossiers.
{: .text-justify}

## Une Grille sur notre MAP
Nous allons découper notre map de **nb_spritesY * nb_spritesX tuiles** (chaque tuiles étant une image carrée de size_spriteY x size_spriteX pixels) en une grille de petits carrés qui vont servir de base à Pygame pour gérer les collisions, en considérant que chaque carré sur la grille est soit franchissable, soit infranchissable. Plus les carrés seront fin, et plus la détection de collision sera réaliste. C'est pour cette raison que le nombre de carrés sur notre grille sera plus grand que le nombre de tuiles. Découpons chaque tuile en **nb_cuts x nb_cuts blocs carrés**, par exemple avec nb_cuts=2 on obtient des blocs carrés de 32x32 pixels quand les tuiles font 64x64 pixels: ce sera la définition de notre grille dans ce tutoriel. Nous avons donc une grille à construire composée de nb_spritesY.nb_cuts x nb_spritesX.nb_cuts blocs qui seront d'un taille de size_spirteY/nb_cuts x size_spriteX/nb_cuts pixels chacun. 
{: .text-justify}
> Il est très pratique de prévoir des tailles de **tuile carrée** et d'une **puissance de 2**: (64x64 pixels dans notre cas = 2⁶x2⁶), et d'envisager un nombre de coupes nb_cuts en puissance de 2 aussi (nb_cuts=2 dans notre cas, sinon 4 ou 8 pour plus de finesse ... mais éviter 3,5,6,7): comme ça avec des divisions par nb_cuts on n'a aucun problème d'arrondi.
{: .text-justify}
![Pygame](/assets/images/tutos/035Pygame/pygame08.png){: .align-left}
Pour les agents qui sont en définition carrée **size x size pixels** (128x128 pixels dans notre cas) on pourrait appliquer le même principe d'une grille, mais je vais simplifier l'approche en considérant qu'à l'intérieur du carré size x size pixels il y a **un seul carré centré** de taille bloc_size x bloc_size pixels (carré bleu dans l'image ci-contre) qui est infranchissable, le reste correspondant à la zone de transparence de l'image ou décors qui peuvent être affichés en superposition avec le reste (comme la zone du bouclier représentée par en cercle bleu) . Mais si on veut gérer les collisions encore plus finement, on pourrait remplacer ce bloc unique par une grille. Comme notre agent peut être orienté dans toutes les directions à l'intérieur de ce bloc infranchissable: je considère que toute la zone de ce seul bloc est infranchissable.
{: .text-justify}

Afin de gérer ces nouveaux éléments sur notre map et nos agents, nous allons revoir les définitions de notre **Classe Terrain()**, ainsi que les **classes Agent()** et leurs dérivées.
{: .text-justify}

## Classe Terrain()
### redéfinition des dictionnaires de tuiles
Souvenez-vous dans le [tutoriel n°2](https://www.papsdroid.fr/post/jeux-pygame-partie02) pour apprendre à créer un décors à partir de tuiles 64x64 pixels, nous avons codé en dur dans la **classe Terrain()** le dictionnaire self.sprites qui associe un code unique de tuile à un fichier image. Ce dictionnaire est ensuite utilisé pour construire la MAP en lisant les fichiers *.map dans le dossier Map/
{: .text-justify}
```python
# dictionnaire des tuiles utilisées dans la MAP { 'CODE': 'fileName'}
self.sprites= 
{'  ': None,   
  '00': 'Ground_Tile_01_A_64x64.png', # fond désert 1
  '01': 'Ground_Tile_02_A_64x64.png', # fond désert 2
  'h1': 'Hedge_A_01_64x64.png',       # bord haut gauche
  'h2': 'Hedge_A_01b_64x64.png',      # bord haut droite
  'h3': 'Hedge_A_01c_64x64.png',      # bord bas droite
  ...
}
```

Il va falloir maintenant préciser quelles sont pour chacunes de ces tuiles leurs zones franchissables, dans la définition de nb_cuts*nb_cuts blocs à l'intérieur de la tuile. On ne vas pas le coder en dur, mais plutôt passer par un fichier texte qui sera chargé avant de construire la MAP: il s'agit du fichier **Map/map_sprites.txt**.
{: .text-justify}
Ce fichier va contenir sur chaque ligne le code de tuile sur 2 caractères, puis le nom de l'image, un commentaire explicatif, et les nb_cuts x nb_cuts (4 dans notre cas) indicateurs d'occupation dans l'ordre de gauche à droite, puis du haut vers le bas. Un 'x' signifie que le bloc est infranchissable. Chaque donnée est séparée par un '|', et on peut ajouter des commentaires en démarrant une ligne avec '#'. 
{: .text-justify}
```python
# ZONE DE COMMENTAIRE NON PRISE EN COMPTE
|00|Ground_Tile_01_A_64x64.png|fond désert 1                  |  |  |  |  |  |01|Ground_Tile_02_A_64x64.png|fond désert 2                  |  |  |  |  |  |h1|Hedge_A_01_64x64.png      |bord haut gauche               |x |x |x |  |  |h2|Hedge_A_01b_64x64.png     |bord haut droite               |x |x |  |x |
...
```

La méthode **self.read_sprites()** va lire et construire le dictionnaire de tuiles **self.sprites**, ainsi qu'une liste de nb_cuts x nb_cuts dictionnaires pour stocker le code occupation de chaque tuile pour chacun des blocs (c'est
 là qu'il faut être à l'aise avec des listes et dictionnaires Python ...).
{: .text-justify}
Pour traquer les anomalies d'incohérences avec des formatages non respectés, et des noms de fichiers inconnus, j'utilise la gestion des exceptions native de Python, ce qui fait que pas mal de code est encadré par les mots clés **try:** et **except:** Je ne voulais pas sur-complexifier le code avec un interpréteur complexe donc c'est perfectible, mais bien efficace. Le squelette ressemble à ceci:
{: .text-justify}
```python
try:
   with open(monfichier, 'r') as f:
      try:
         ... code python qui interprète le fichier 'monfichier'
      except:
         print('problème avec le formatage de', monfichier)
         exit()
except:
   print('problème ouverture de ', monfichier)
   exit()
```

Les retours de cette méthode self.read_sprites() sont donc:
{: .text-justify}
* **self.sprites**: dictionnaire des tuiles. Ainsi self.sprites['code'] stocke le nom de l'image de la tuile stockée dans Map/ et référencée par le code 'code' dans les fichiers *.map
{: .text-justify}
* **self.l_sprites_grid** est une liste de nb_cuts*nb_cuts (4 dans notre cas) dictionnaires, chaque dictionnaire regroupe pour chaque code de tuile 'code' un indicateur booléen True/False pour savoir si la tuile est franchissable pour ce bloc. self.sprites_grid[n]['code'] renvoie donc True ou False selon que la tuile référencée par 'code' a son bloc 'n' franchissable ou non.
{: .text-justify}
une fois qu'on a constitué ces dictionnaires, nous pouvons lire les fichiers *.map et construire non seulement le décors en background, mais aussi la grille complète sur la map avec les indicateurs d'occupation True/False pour savoir s'ils sont franchissables ou non.
{: .text-justify}

### Construction de la map et de sa grille
Pour construire la map en background, c'est déjà bien expliqué dans le tutoriel n°2: on fabrique d'abord un dictionnaire d'images au format PIL **self.sprites_pil** à partir du dictionnaire des fichiers d'images self.sprites.
{: .text-justify}
La méthode **self.read_map(liste_fichiers_map)** va lire les fichiers *.map fournie en paramètre dans une liste, et donner en retour: 
{: .text-justify}
* **matrix_map_pil**: une matrice (liste en 2 dimensions) d'images PIL correspondants aux nb_spritesY * nb_spritesX tuiles à afficher pour notre background. matrix_map_pil[y][x] correspond à l'image au format PIL de la tuile en ligne y et colonne x. 
{: .text-justify}
* **self.l_matrix_occup**: une liste de nb_cuts x nb_cuts matrices en 2 dimension (c'est donc une matrice en 3 dimension en fait...). Chaque matrice 2d stocke les nb_spritesY x nb_spritesX codes occupation True/False de chaque bloc de chaque tuile. self.matrix_occup[n][y][x] = True ou False: il s'agit du code occupation du bloc n de la tuile en ligne y et colonne x.
{: .text-justify}
Cette lecture de liste de fichiers map se fait en plusieurs temps pour initialiser ces matrices 2d et 3d dans un premier temps, puis en fusionnant ensuite les images de tuiles les unes sur les autres d'une part, et en enregistrant les codes occupation à True d'autre part.
{: .text-justify}
Enfin, à partir de cette liste de matrices d'occupation de chaque bloc sur la grille de tuiles, nous construisons la grille finale **self.map_occup** qui est une matrice en 2 dimensions qui stocke le code occupation True/False de chaque bloc sur une grille de blocs de dimension nb_cuts x nb_spritesY lignes et nb_cuts x nb_spritesX colonnes. self.map_occup[l][c] = True/False correspond au code occupation sur la grille de blocs en ligne l et colonne c.
{: .text-justify}
La méthode **self.isFull(grid_pos)** renvoie le code occupation True/False en position grid_pos=(l,c) depuis cette grille: c'est elle qui sera appelée par les agents pour savoir si l'espace est occupé ou non, afin de gérer les collisions avec le décors.
{: .text-justify}

## Classe Agent()
En rappel du [tutoriel n°4](https://www.papsdroid.fr/post/jeux-pygame-partie04), notre **classe Agent()** gère tous les aspects déplacements des agents sur la map. C'est donc ici qu'il faut coder des méthodes de détection des blocs qui sont en collision avec notre agent. 
{: .text-justify}
> Dans ce tutoriel je n'aborde uniquement que la **détection des collisions**: nous verrons dans un autre tutoriel comment réagir face à une collision.
{: .text-justify}

Pour définir le bloc infranchissable interne de l'agent, utilisons la variable **self.bloc_size**. Ensuite nous dotons dans un premier temps nos agents de la méthode **self.pos_to_grid()** qui va convertir la position self.pos de l'agent en ligne et colonne sur la grille de blocs du terrain. Les agents ayant une taille bien supérieures aux blocs, on ne retourne que les coordonnées (l,c) du 1er bloc dans lequel le 1er pixel en haut à gauche du bloc interne infranchissable de l'agent se situe. 
{: .text-justify}

![Pygame](/assets/images/tutos/035Pygame/pygame09.png){: .align-left}
C'est à partir de ce 1er bloc sur la map fourni par self.pos_to_grid() que l'on va détecter s'il y a collision où non, mais il n'est pas le seul impliqué au vu de la taille des agents et du fait qu'il peuvent se balader n'importe où sur la map, pixel par pixel. La méthode **get_neighbour_blocks()** va construire une liste de rectangles Pygame qui correspondent aux blocs de la map infranchissables qui viennent se percuter avec la zone infranchissable de notre agent. La méthode comment par le 1er bloc en (l_stat, c_start) fournie par self.pos_to_grid() puis quadrille la map de gauche vers la droite puis de haut vers le bas sur un nombre de blocs voisins en recouvrement qui est toujours égal à int(self.bloc_size/self.terrain.bloc_size)+1. Ainsi avec nos agents "tank" dont le bloc_size est en 64x64 pixels et des blocs de map en 32x32 pixels on obtient un quadrillage de 3x3 blocs dont il faut vérifier s'ils sont franchissables ou non. La méthode ne rajoute dans la liste que les rectangles Pygame correspondants aux bloc infranchissables.
{: .text-justify}
Enfin la méthode **self.bouge()** de l'agent a été adaptée pour afficher en bleu la zone infranchissable de l'agent, et afficher en rouge les rectangle en collision avec le décors (bien entendu pour le jeux final on va retirer cet affichage ...)
{: .text-justify}
```python
#dessin des blocs en collision et de la zone infranchissable de l'agent for rect in blocks: #dessins de rectangle rouge sur les blocs en collision
   pygame.draw.rect(self.terrain.screen, (255,0,0), rect,3) pygame.draw.rect(self.terrain.screen, (0,0,255), self.rect, 3)
```
>Si vous exécuter le programme **tank05.py** (commandes clavier: 'S' et 'D' pour pivoter le canon, 'F' pour tirer un obus, 'ESPACE' pour activer/désactiver le bouclier, flèches haut/bas pour accélérer/freiner; flèches droite/gauche pour pivoter le tank) vous verrez les cadres bleus et rouges s'afficher en fonction du franchissement des obstacles, idem si vous tirez un obus et qu'une explosion se produit.
{: .text-justify}

## Plus le quadrillage est fin, et meilleur sera le rendu
{: .text-justify}
Dans les programmes fournis je suis parti avec un découpage des tuiles de 64x64 pixels en 2x2 blocs de 32x32 pixels chacun donc. J'ai donc 2x2=4 codes occupation dans mon fichier texte Map/map_sprites.txt. On peut voir dans les situations ci-dessous que ce n'est pas suffisamment fin pour gérer certains décors: il faudra donc envisager un découpage en 4x4 = 16 blocs au lieu de 2x2= 4 blocs.
{: .text-justify}
![Pygame](/assets/images/tutos/035Pygame/pygame10.png){: .align-center}
* **1er cas**: la zone infranchissable du tank bute sur un tronc qui est en plein milieu d'une tuile 64x64 pixels mais plus fin que 32 pixels: en conséquence la détection de collision se fait trop tôt. Pour y remédier il faut découper les tuiles en 4x4 blocs et non pas 2x2
{: .text-justify}
* **2ème cas**: idem que le 1er sur le bord du cadre très fin: une résolution en 4x4 blocs permettra de s'approcher au plus près avant de détecter une collision. Si vous êtes furieux vous pouvez aller jusqu'à décrire les 8x8=64 codes occupation de chaque tuile !
{: .text-justify}
* **3ème cas**: on n'y peut rien! le système de collision fonctionne avec des zones rectangulaires et notre agent peut pivoter à l'intérieur de sa zone: il y aura donc des moment où une collision empêchera notre agent d'avancer alors qu'il pourrait. C'est la limite d'un système de détection de collision par rectangles.
{: .text-justify}
* **4ème cas**: heu non c'est parfait! on voit bien que le tank roule sur un décors (un cactus) qui a été déclaré "franchissable" dans notre fichier Map/map_sprites.txt.
{: .text-justify}

>A noter que sur un **Raspberry pi4 4Go**, l'animation avec détection en temps réel des collisions avec les décors dans un jeux animé avec 30 FPS et quelques agents (2 tanks, 2 obus, 2 explosions) ne prend qu'à peine 20% des ressources CPU: pygame est vraiment bien optimisé !
{: .text-justify}