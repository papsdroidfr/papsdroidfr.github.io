---
#source: https://www.papsdroid.fr/post/jeux-pygame-partie06
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
  teaser : "/assets/images/tutos/035Pygame/pygame11.png"

#SEO tags
title       : "Coder un jeux avec Pygame - 06 détecter les collisions au pixel près !"
image       : "/assets/images/tutos/035Pygame/pygame11.png"
description : "Ceci est une alternative au tutoriel précédent qui explique comment détecter des collisions à partir de rectangles qui quadrillent la map. Cette fois nous allons affiner notre détection pour qu'elle se fasse au pixel près."

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Pygame"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "DEV" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["python3", "Pygame" ]

gallery1:
  - image_path: "/assets/images/tutos/035Pygame/pygame14.png"
  - image_path: "/assets/images/tutos/035Pygame/pygame15.png"
  - image_path: "/assets/images/tutos/035Pygame/pygame16.png"
---

![Pygame](/assets/images/tutos/035Pygame/pygame11.png){: .align-left}
Ceci est une alternative au [tutoriel précédent](https://papsdroidfr.github.io/dev/Pygame-detection-collision/) qui explique comment détecter des collisions à partir de rectangles qui quadrillent la map. Cette fois nous allons affiner notre détection pour qu'elle se fasse **au pixel près**, et non plus uniquement à partir de recouvrement de zones rectangulaires.
{: .text-justify}

## Avantages et inconvénients de la méthode des rectangles
Souvenez-vous pour faire notre détection de collisions à partir de rectangles, nous avions quadrillé le terrain de 2x2 blocs carrés sur chaque tuiles de 64x64 pixels. La détection de collisions vérifiait alors le chevauchement de la zone infranchissable d'un agent avec les blocs qu'il traverse sur le terrain. Si cette méthode a l'avantage d'être très rapide (peu gourmande en CPU), elle présente les inconvénients suivants:
{: .text-justify}
![Pygame](/assets/images/tutos/035Pygame/pygame12.png){: .align-left}
Pour pouvoir s'approcher au plus près d'un objet sans déclencher de collision, il faut découper plus finement les tuiles en 4x4 = 16 blocs, voire 8x8 = 64 blocs ! ce qui rend vraiment compliqué l'écriture du fichier qui décrit les tuiles (map_sprites.txt) avec leurs zones franchissables/non franchissables. Tant qu'il n'y a que 2x2=4 blocs à décrire pour chaque tuile ça peut aller, mais au delà ça pique les yeux...
{: .text-justify}
![Pygame](/assets/images/tutos/035Pygame/pygame13.png){: .align-left}
La méthode des rectangles est incapable de gérer cette situation où les deux zones se chevauchent, mais les images n'occupant pas tout l'espace ne se chevauchent pas: une collision sera pourtant bel et bien détectée dans ce cas, et peu importe le nombre de blocs de découpe pour chaque tuile !
{: .text-justify}

>Pour remédier à ces deux problèmes, nous allons appliquer une méthode de détection de collision "grossière" avec des rectangles 64x64 pixels sans aucun sur-découpage (afin de limiter la zone de recherche de collision), et affiner ensuite au pixel près avec des masques d'images.
{: .text-justify}

## Détection de collision avec des masques
Pygame permet de détecter au pixel près si les pixels non transparents de deux zones rectangulaires se chevauchent. Pour cela il faut calculer les masques des images. Un masque c'est comme une image binaire où les pixels non transparents valent 1, tandis que les pixels transparents valent 0. 
{: .text-justify}
Pour générer un masque à partir d'une image (sous-entendu notre image possède un fond transparent):
{: .text-justify}
```python
img= Image.open(img_file)
mask = pygame.mask.from_surface(img)
```

Pour détecter si deux masques 'mask1' et 'mask2' sont en collision au pixel près, sachant que leurs rectangles respectifs sur le terrain est 'rect_mask1' et 'rect_mask2', la syntaxe ressemble à ceci:
{: .text-justify}
```python
if mask1.overlap( mask2,
        ( rect_mask2.left - rect_mask1.left, 
          rect_mask2.top - rect_mask1.top 
        )                             
      ) != None :
    print('collision!')
else:
    pass
```

Bien évidemment cette méthode de détection de collision au pixel près est plus lourde (du point de vue CPU) que de se contenter de vérifier si deux rectangles se chevauchent. Mais si on est malin, on pré-calcule à l'avance tous les masques une fois pour toutes, et on limite la recherche d'overlap de masques uniquement dans la région proche des agents: raison pour laquelle il faut dans un premier temps repérer les blocs du terrain qui sont en chevauchement avec le bloc de l'agent, et ne tester les overlap de masques (pré-calculés) uniquement pour les blocs qui contiennent des éléments de terrain "infranchissables".
{: .text-justify}
>Ci-dessous, les blocs fin surlignés en jaune sont des blocs qui contiennent des éléments infranchissables. Lorsque les blocs jaunes deviennent épais: ils sont en chevauchement avec le bloc d'un agent, ce qui active la recherche d'overlap. Enfin s'ils deviennent tout rouge c'est que l'overlap a détecté des pixels non transparents qui se chevauchent.
{: .text-justify}
{% include gallery id="gallery1" caption="détection collision au pixel près" %}

## Les programmes Python de ce tutoriel:
A récupérer sur [mon Github](https://github.com/papsdroidfr/Tuto_Pygame_Tank), il s'agit du dossier **\TANK06** à récupérer dans son intégralité.
{: .text-justify}
Il faut exécuter le programme **tank06.py**. Le tank se contrôle avec les flèches du clavier, puis les touches 'S', 'D' pour orienter la tourelle, 'F' pour tirer, 'ESPACE' pour activer/désactiver le bouclier. 
{: .text-justify}
La réaction aux collisions n'est pas encore codée, donc c'est normal que le tank traverse les murs ... Ce sera l'objet du prochain tutoriel.
{: .text-justify}
### La classe Terrain() 
Elle a été simplifiée puisqu'on ne va plus chercher à "sur-quadriller" le terrain avec n*n blocs pour chaque tuiles, mais avec un nombre de bloc égal au nombre de tuiles. Au même titre que l'on "fusionne" les tuiles des map pour fabriquer le background final, on va fusionner les blocs infranchissables afin de générer les masques. 
{: .text-justify}
Les dictionnaires et listes importants à bien comprendre dans cette classe sont:
{: .text-justify}
* **self.sprites** = dictionnaire des tuiles { 'CODE': 'fileName.png'}
{: .text-justify}
* **self.sprites_grid** = dictionnaire des zones d'occupations des tuiles  {'CODE' : TRUE/FALSE} True signifie que la tuile contient un élément infranchissable.
{: .text-justify}
* **self.sprites_pil** : dictionnaire des images de tuiles au format PIL
{: .text-justify}
* **matrix_pil_background[l][c]** = image 64x64 pixels ligne l colonne c sur le terrain
{: .text-justify}
* **matrix_pil_bloc[l][c]** = image 64x64 pixels ligne l colonne c concernant un bloc infranchissable sur le terrain. 'None' si la zone est franchissable.
{: .text-justify}
* **self.matrix_bloc[l][c]** = True/False , True = bloc infranchissable
{: .text-justify}
* **self.matrix_mask[l][c]** = masque des tuiles en ligne l et colonne c, 'None' si zone entièrement franchissable.
{: .text-justify}

### La classe Agent()
Pour simplifier l'approche, on va convenir qu'une seule image de l'agent (self.id_img_mask) ne va faire l'objet de tests d'overlap. Dans le cas de notre agent Tank il s'agit du corps du tank. Cela provoquera une incohérence lorsque le corps du tank frôlera sans le toucher un mur, tandis que sa tourelle qui peut dépasser du corps sera orientée vers le mur: elle sera "dans" le mur sans qu'aucune collision ne soit détectée. Pour y remédier il faudrait généraliser la méthode à toutes les images de l'agent sauf celles qui peuvent s'afficher partout (exemple la zone de protection du bouclier). 
{: .text-justify}
Les seules grandes différences avec le tutoriel précédent concernent le pré-calcul des masques toutes orientations confondues (self.nb_rotates masques) lors du constructeur def __init__(self) de la classe, et la méthode def get_neighbour_blocks(self): qui va détecter les blocs qui se chevauchent exactement comme au tutoriel précédent, et ne retenir que les blocs pour lesquels les overlap de masques ne sont pas nuls.
{: .text-justify}
Les listes importantes à bien comprendre:
{: .text-justify}
* **self.l_img_rotated[n][i]** = image n en rotation i.nb_rotates/360 °
{: .text-justify}
* **self.l_sin[i]** = sinus(i.nb_rotates/(2pi))
{: .text-justify}
* **self.l_cos[i]** = cosinus(i.nb_rotates/(2pi))
{: .text-justify}
* **self.l_img_mask[i]** = masque de l'image sefl_id_img_mask, en rotation de i.nb_rotates/360 °
{: .text-justify}

>Avec cette technique, le surcoût CPU s'avère insignifiant puisque je reste à **22% de CPU (sur un Raspberry pi4)** exactement comme avec la technique du tutoriel précédent. Mais les choses peuvent se corser à mon avis si l'on multiplie fortement les agents qui se baladent à l'écran, ainsi que les zones non franchissables du terrain. En fonction de la finesse de détection de collision souhaitée et du nombre d'agents qui interagissent, il faudra faire un choix.
{: .text-justify}