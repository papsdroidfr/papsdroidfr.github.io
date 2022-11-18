---
#source: https://www.papsdroid.fr/post/jeux-pygame
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
  teaser : "/assets/images/tutos/035Pygame/logoPygame.png"

#SEO tags
title       : "Apprendre à coder un jeux avec Pygame: 01 les bases"
image       : "/assets/images/tutos/035Pygame/logoPygame.png"
description : "Une occupation vraiment sympa à faire avec un Raspberry pi c'est d'apprendre à coder en Python. Je vous propose ici une série d'articles pour apprendre à coder un sympathique jeux avec le moteur de jeux Pygame"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Pygame"

# category: "tutoriels" "configuration" "IA" "game" "aquapi" "planétaire" 
category    : "game" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["python3", "Pygame" ]

---
![Pygame](/assets/images/tutos/035Pygame/logoPygame.png){: .align-left}
Une occupation vraiment sympa à faire avec un Raspberry pi c'est d'apprendre à **coder en Python**. Je vous propose ici une série d'articles pour apprendre à coder un sympathique jeux avec le moteur de jeux [Pygame](https://www.pygame.org/news), l'objectif étant de tout faire depuis un Raspberry pi, mais ça fonctionnera aussi sur n'importe quelle autre plate-forme.
{: .text-justify}

## A quoi sert Pygame?
On peut faire beaucoup de choses avec Pygame, y compris des jeux en 3D mais son domaine de prédilection reste vraiment le **moteur de jeux 2D** accessible via une bibliothèque Python. Mais au fait c'est quoi un "Moteur de jeux 2D"? C'est un ensemble de fonctions (en l’occurrence écrit dans une bibliothèque Python) qui va permettre de gérer l'affichage de décors (images) dans une fenêtre, de gérer l'animation des décors avec toute une mécanique de gestion des collisions, de gérer aussi tout l’aspect multimédia (les sons par exemple) et surtout de gérer aussi les événements qui vont permettre d’interagir via un clavier, une souris, une manette, un joystick etc ... L'avantage de passer par un tel moteur est qu'on s'affranchi de devoir coder tous ces aspects, et on peut se **concentrer sur le gameplay** du jeux qu'il va falloir coder en Python.
{: .text-justify}

## Le gameplay proposé dans ce tutoriel
![Pygame](/assets/images/tutos/035Pygame/LogoTANK.jpg){: .align-left}
Si comme moi vous avez une recalBox pour émuler des jeux old-school en 2D les idées devraient fuser ... mais nous allons devoir nous concentrer dans ce tutoriel sur un gameplay. Je propose de partir sur un style jeux de bataille 2D où des **agents** contrôlés par le joueur et par logiciel évoluent dans un décors composé de **tuiles** qui composent notre fenêtre. Ici en l’occurrence nos agents seront des tanks qui vont évoluer dans un décors adéquat. 
{: .text-justify}

Les notions importantes à retenir sont: 
* **Le background** (fond d'écran) est composé de tuiles de tailles identiques (64*64 pixels)
{: .text-justify}
* **une tuile** est une petite image 64*64 pixels (ou autre) avec un fond transparent
{: .text-justify}
* **Les agents** sont aussi composés de tuile(s) qu'on va pouvoir animer, et qui vont interagir avec le décors et entre eux.
{: .text-justify}

Il va falloir dans un premier temps récupérer des tuiles libre de droits, ou bien acheter des tuiles professionnelles, ou encore se les créer soit-même. Je vous encourage à jeter un œil sur ce splendide dataset de tuiles chez [craftpix](https://craftpix.net/) dont certaines sont libres de droit: c'est ce que j'utilise donc dans ce tutoriel: les images utilisées sont bien libres de droit pour toute utilisation, même commerciale.
{: .text-justify}

>Vous pouvez suivre ce tutoriel pas à pas avec le même jeux de tuiles, ou bien vous en inspirer pour faire votre propre jeux dans un tout autre univers (exemple: jeux d'aventure "Zelda-like") car les principes seront les mêmes.
{: .text-justify}

## Première étapes: installer la bibliothèque pygame
ultra simple sur son Raspberry, et on en profite pour une mise à jour au passage (bonne pratique). Dans le terminal de commande tapez ces 3 commandes
{: .text-justify}
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python-pygame
```
et c'est tout !

pour tester, exécutez en ligne de commande le code suivant:
{: .text-justify}
```python
python3
import pygame
```

vous devriez voir alors la version de pygame installée, ainsi qu'un sympatrique message de bienvenue "Hello from the pygame community. https://www.pygame.org/contribute.html" 
{: .text-justify}
dans ce cas c'est tout bon !
{: .text-justify}
pour sortir de python il faut appuyer sur les touches : CTRL D
{: .text-justify}

## Premier Tutoriel: génération du background et contrôle d'un agent avec la souris
Nous allons commencer très simplement par comprendre comment générer un background composé de tuiles de 64*64 pixels, et comment y faire évoluer un agent composé d'une seule tuile (corps d'un tank) 64*64 pixels aussi, qui sera contrôlé avec la souris. A l’heure où j’écris cet article, seul ce premier tuto est prêt: je rédigerai des articles complémentaires qui vont aller de plus en plus loin, jusqu'au jeux dans son intégralité.
{: .text-justify}

### prérequis:
* modèle conseillé de Raspberry pi: pi3b+ ou pi4 (j'utilise un pi4 avec 4Go de RAM pour info)
{: .text-justify}
* avoir installé la bibliothèque Pygame (voir explications plus haut)
{: .text-justify}
* Nous allons coder en Python: si vous n'avez jamais codé dans ce langage ça va être raide pour tout comprendre. Je vous encourage dans ce cas de suivre ce superbe cours gratuit sur [OpenClassroom](https://openclassrooms.com/fr/courses/7168871-apprenez-les-bases-du-langage-python?archived-source=235344).
{: .text-justify}
* Je suis ce qu'on appelle un "gourou de la POO" (acronyme de Programmation Orientée Objet), sans doute du à mon héritage C++ et Java, donc tous mes programmes sont 100% conçus orientés objets. Je trouve que la conception orientée objet est très agréable et plus simple à maintenir, surtout dans notre cas où l'on va devoir programmer l’interaction d'agents autonomes avec leur environnement qui évolue suite aux actions des agents. Si vous connaissez Python mais que vous n'avez jamais développé Orienté Objet je vous conseille alors de faire un tour sur la formation d'OpenClassroom ci-dessus qui aborde bien cet aspects, sans quoi vous ne comprendrez rien à ces phrases "instancier un objet", "constructeur de l'objet", "héritage" etc ... Mais je vous rassure, ça va rester très simple.
{: .text-justify}

Maintenant que la bibliothèque pygame est bien installée sur votre Raspberry pi préféré, allez récupérer le code Python de cette première partie du tutoriel sur [ma page Github](https://github.com/papsdroidfr/Tuto_Pygame_Tank): il faut récupérer le dossier "Tank01" et assurez vous que vous avez bien récupéré aussi le sous-dossier "Media" qui contient les tuiles 64*64 utilisées au format .png
{: .text-justify}

Ce petit programme de 74 lignes (commentaires inclus) montre les bases de notre jeux écrit en pygame.
{: .text-justify}
### bibliotèques
> On commence par importer les bibliothèque dont nous avons besoin:
{: .text-justify}
```python
import pygame                   # PYGAME package
from pygame.locals import *     # PYGAME constant & functions
from sys import exit            # exit script 
```

### classe Game()
>Ensuite nous définissons une classe **Game()** qui va contenir 3 méthodes:
{: .text-justify}

**1) constructeur de la classe**: c'est la méthode qui est automatiquement exécutée lorsqu'on instancie un objet. On en profite pour y gérer les initialisations de notre objet et sauvegarde de variables toutes raccrochées au pointeur sur l'objet lui même "self" afin que les autres méthodes de l'objet puisse accéder à ces variables. C'est toute la puissance de la programmation orientée objet: chaque objet est composé de ses propres variables et de ses propres méthodes qui manipulent ces variables.
{: .text-justify}

(si vous ne compreniez rien à ces termes barbares, faites un tour sur la formation d'OpenClassroom mentionnée plus haut)
{: .text-justify}
```python
def __init__(self, size_factor_X=18, size_factor_Y=12):
   """ constructeur de la classe
   size_factor_X et size_factor_Y représentent la taille du plateau de jeux
   en nombre de tuiles 64*64 pixels
   """
```
Puisque le background de notre jeux est composé de tuiles de tailles identiques, nous allons utiliser 2 paramètres size_factor_X et size_factor_Y (optionnels car initialisés respectivement à 18 et 12 par le constructeur), afin de représenter le nombre de tuiles totales à utiliser. C'est donc une matrice de 18 * 12 tuiles, chacune faisant 64 * 64 pixels. Je génère ainsi un écran d'un taille de ... 18 * 64 * 12 * 64 = 1152 * 768 pixels = 884 736 pixels. Pas mal non pour notre petit Raspberry pi ? Et il va très bien s'en sortir.
{: .text-justify}
On commence par sauvegarder toutes les variables qui nous seront utiles pour que les autres méthodes puissent y accéder: la taille fixe des tuiles, l'image du background et l'image du tank qui doivent être dans le sous dossier Media, et le nombre de tuiles totales récupérée des 2 variables fournies au constructeur (optionnelles)
{: .text-justify}
```python
    self.size_X, self.size_Y = 64,64   # taille des tuiles 64*64 pixels
    self.background_image_filename = 'Media/Ground_Tile_01_A_64x64.png'   
    self.tank_image_filename = 'Media/Hull_02_64x64.png' 
    self.size_factor_X, self.size_factor_Y = size_factor_X, size_factor_Y
```

Ensuite viennent les initialisation de notre fenêtre de jeux Pygame, en définissant la taille de l'écran calculé à partir du nombre de tuiles et de la taille des tuiles et un jeux de couleur de 32bits. On ajoute un petit titre à notre fenêtre "TANK - Pygame Tutorial PARTIE 01", et nous chargeons nos 2 images de background et de tank dans 2 objets image fournis par la méthode pygame.image.load
{: .text-justify}
```python
pygame.init()  # initialisation Pygame
self.screen = pygame.display.set_mode((self.size_X*self.size_factor_X, self.size_Y*self.size_factor_Y),0,32)
pygame.display.set_caption("TANK - Pygame Tutorial PARTIE 01") 
self.background = pygame.image.load(self.background_image_filename).convert()   
self.tank_tile = pygame.image.load(self.tank_image_filename).convert_alpha()
```

**2) boucle principale de la classe:** il s'agit de la boucle infinie de lecture des événements et qui ne prend aucun paramètre: **def loop(self)**. la seule variable obligatoire en POO Python est le pointeur sur l'objet lui-même (self) qui va permettre à notre méthode nommée "loop" de récupérer les variables de l'objet. Le corps principal de cette méthode est donc une boucle infinie incluse dans un bloc while True:
{: .text-justify}
La boucle commence par lire les événements pygame: c'est un bon moyen pour capter les actions du joueur et réagir à ses interactions. Dans ce tutoriel un seul événement est capté: est-ce que le joueur a cliqué sur la petite croix en haut à droite de l'écran ? Si oui, on appelle direct le destructeur de l'objet self.destroy() qui va mettre fin à la partie.
{: .text-justify}
```python
for event in pygame.event.get():  
    if event.type == QUIT:  # evènement click sur fermeture de fenêtre
        self.destroy()    
```

Ensuite le rôle de notre boucle infinie est aussi de rafraîchir l'écran de jeux qui est composé d'une série de tuiles pour le background + la tuile de notre agent "tank". Pour montrer comment interagir avec l'environnement, je vais positionner le tank par rapport à la position de la souris.
{: .text-justify}
Cette partie du code balaye l'écran selon le nombre de tuiles (le pas utilisé est donc la taille d'un tuile) et calcule en conséquence les positions (x,y) respectives où il faut déposer notre tuile background à l'écran. Pour cela on utilise la méthode **blit()** de notre objet self.screen initialisé par le constructeur, avec en paramètre l'image à utiliser (self.background, initialisé dans le constructeur aussi) et les coordonnées (x,y) où déposer cette image. A noter que la méthode blit() ne dessine rien du tout à l'écran: elle prépare en mémoire en fait le prochain rafraîchissement qui sera fait.
{: .text-justify}
```python
for x in range(0, self.size_X*self.size_factor_X, self.size_X):
    for y in range(0, self.size_Y*self.size_factor_Y, self.size_Y):
        self.screen.blit(self.background,(x,y))
```

Cette partie du code va s'occuper d'afficher ensuite (par dessus le background donc) la tuile correspondante à notre agent "tank". Pour constater qu'il peut se déplacer, récupérons les coordonnées de la souris dans (x,y) qu'on utilise alors pour positionner le tank à l'écran: c'est super simple à coder et pas besoin de se prendre la tête à savoir si la souris est en dehors de l'écran ou pas: le moteur de jeux pygame gère tout ça pour nous!
{: .text-justify}
```python
x,y = pygame.mouse.get_pos()             # récupère la position de la souris
x-= self.tank_tile.get_width() / 2       # recadrage sur le milieu du tank 
y-= self.tank_tile.get_height() / 2         
self.screen.blit(self.tank_tile, (x,y))  # tuile tank en position (x,y)
```

Enfin la dernière ligne de code va provoquer le rafraîchissement de l'écran qui a été préparé par les méthodes .blit().
{: .text-justify}
```python
pygame.display.update() # rafraîchi l'écran
```

Et c'est tout, nous avons un gameplay très sommaire mais qui permet de bien comprendre les grands principes de pygame pour le moment. Je vous rassure on va améliorer tout ça dans les prochains tuto ;-)
{: .text-justify}

>**Et mais?!** attends une minute... tu nous dis de répéter en boucle infinie de redessiner tout le backgound à l'écran alors que c'est tout le temps le même: ça ne sert à rien non ?

Ha mais si, **il faut constamment le redessiner ce background** même s'il reste figé! En effet le tank lui va bouger sur l'écran: si on ne redessine pas le backgound on verrait la trace du tank partout à l'écran au fur et à mesure qu'on le fait bouger. Et puis plus tard quand ils seront plusieurs tanks à se tirer dessus ça va bouger et exploser dans tous les sens...
{: .text-justify}

Pour ne pas se poser de question de qu'est ce qui a bougé à l'écran: on redessine tout dans cet ordre: background complet d'abord, et agents ensuite.
{: .text-justify}

**3) destructeur de la classe:** cette méthode def destroy(self): va permettre de quitter proprement le programme pour gérer par exemple d’éventuelles sauvegardes, fermer les fenêtres, tuer tous les process qui tournent en boucle infinie. Dans notre premier tuto elle est très simple: un petit message "Bye!", suivi de la fermeture de la fenêtre de jeux, et d'un exit() qui tue tous les process: fin du jeux.
{: .text-justify}
```python
    def destroy(self):
        """
        destructeur de la classe
        """
        print('Bye!')
        pygame.quit() # ferme la fenêtre principale
        exit()        # termine tous les process en cours
```
### coprs du programme
**Le corps du programme** consiste juste à instancier un objet de la classe Game(), d'exécuter sa boucle infinie loop() tout en interceptant un éventuel CTRL-C au clavier pour provoquer la destruction de l'objet.
{: .text-justify}

**appl=Game()** provoque l'instanciation d'un objet de la classe Game(): à ce moment il faut comprendre que le constructeur __init__() de notre objet est automatiquement exécuté sans aucun paramètre. Vous pouvez par exemple modifier cette ligne en appl=Game(15,10) et dans ce cas le nombre de tuiles utilisées sera de 15 * 10 au lieu des valeurs par défaut 18 * 12 du constructeur.
{: .text-justify}
Ensuite l'interruption clavier CTRL-C se gère par interception d’exceptions: on exécute la boucle infinie appl.loop() de notre objet de type Game() et si l'utilisateur tape CTRL-C au clavier ça aura le même effet que s'il clique sur la croix "quitter" en haut de l'écran: appel au destructeur appl.destroy() !
{: .text-justify}
```python
if __name__ == '__main__':
   appl=Game() #instanciation d'un objet de la classe Game()
   try:
       appl.loop()
   except KeyboardInterrupt: # interruption clavier CTRL-C: appel à la méthode destroy() de appl.
       appl.destroy()
```

Et voilà, à l’exécution du pgm vous devriez voir apparaître ce décors dans lequel se ballade un tank avec notre souris, sur une définition d'image en 1152 * 768 pixels , 32bits. 
{: .text-justify}
![Pygame](/assets/images/tutos/035Pygame/pygame01.png){: .align-center}
>**Whoua!** mais il est trop mortel ce jeux, non ?!

Haha! trêve de plaisanterie ce n'est que la première partie du tutoriel qui permet d'aborder la programmation (orientée objet) avec Pygame, et mine de rien notre petit Raspberry pi arrive à faire tout seul comme un grand:
{: .text-justify}
* générer un background composé 18 * 12  tuiles de 64 * 64 pixels couleur 32bits
* animer un tank à l'écran positionné par la souris du joueur
* lire en boucle infinie les événements afin de réagir aux click "fin de partie" dans notre exemple

> La prochaine partie du tutoriel va consister à voir comment on peut composer le background avec plusieurs tuiles différentes, afin de générer un décors digne d'un vrai jeux de bataille ! A suivre donc...

* Partie 02: [gestion des décors en background](https://www.papsdroid.fr/post/jeux-pygame-partie02).
* Partie 03: [déplacement avec rotation des agents](https://www.papsdroid.fr/post/jeux-pygame-partie03).
* Partie 04: [animation des agents](https://www.papsdroid.fr/post/jeux-pygame-partie04).
* Partie 05: [détection des collisions avec des rectangles](https://www.papsdroid.fr/post/jeux-pygame-partie05)
* Partie 06: [détection des collisions au pixel près !](https://www.papsdroid.fr/post/jeux-pygame-partie06)
