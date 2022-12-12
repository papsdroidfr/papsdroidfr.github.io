---
#source: https://www.papsdroid.fr/post/venus
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
  teaser : "/assets/images/tutos/033Planetaire/venus.jpeg"

#SEO tags
title       : "Planétaire: Vénus"
image       : "/assets/images/tutos/033Planetaire/venus.jpeg"
description : "Après Mercure, qui tourne autour du soleil en 87.967 jours sur notre maquette, place à Vénus qui effectue sa révolution autour du Soleil en 224,700 jours"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Vénus"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["planétaire" ]

gallery1:
  - image_path: "/assets/images/tutos/033Planetaire/venus3D-01.png"
  - image_path: "/assets/images/tutos/033Planetaire/venus3D-02.png"
  - image_path: "/assets/images/tutos/033Planetaire/venus3D-03.png"

---
![Planétaire](/assets/images/tutos/033Planetaire/venus.jpeg){: .align-left}
Après [Mercure](https://papsdroidfr.github.io/tutoriels/Planetaire-Mercure/), qui tourne autour du soleil en **87.967 jours** sur notre maquette (au lieu de 87.969 jours), place à **Vénus** qui effectue sa révolution autour du Soleil en **224,700 jours**. Tous les engrenages vont partir d'un axe commun (celui où l'on retrouve la manivelle grise) qui représente une semaine par tour: tous les mécanismes héritent donc de ce 1er étage de réduction 1/7. Plus tard je vais motoriser tout cela avec un moteur pas à pas qu'il faudra faire tourner d'un angle de 360/7 degrés pour représenter une journée, et donc d'un tour complet par semaine.
{: .text-justify}
Concernant Vénus il faut donc obtenir un rapport de réduction de 224.700 jours, ou encore 32.100 semaines. Ce [programme Python](https://papsdroidfr.github.io/dev/CalculEngrenages/) me permet d'obtenir rapidement les bonnes configurations d'engrenages. J'ai retenu un résultat satisfaisant avec 2 roues menées de 16 dents contre 2 roues menantes de 83 et 99 dents: j'obtiens ainsi (16x16) / (83x99) / 7 = 1 / 224.684 pour un rapport de réduction donc de **224.684** au lieu de 224.700. On peut obtenir une meilleure précision en ajoutant un 3ème étage voire un 4ème étage de réduction, mais la complexité ne vaut pas le gain de précision: rappelons qu'il s'agit d'une maquette mécanique qui n'a pas vocation à faire des calculs de prédiction. Par ailleurs j'ai le même nombre d'étage (2) que pour Mercure, donc les planètes vont tourner dans le même sens: pas besoin de rajouter une roue inverseuse de sens.
{: .text-justify}

## Conception du modèle 3D
{% include gallery id="gallery1" caption="" %}
Chaque mécanisme propre à chaque planète est logé dans des supports horizontaux traversés par des axes hexagonaux qui sont soit en rotation (sur lesquels sont fixés les roues), ou bien fixes car logés dans des trous hexagonaux. Comme les roues ne sont pas identiques selon les planètes (sinon elles auraient toutes la même révolution), il faut jouer sur les deux dimensions horizontales pour loger toutes les roues (seules celles de Mercure sont parfaitement alignées)
{: .text-justify}
![Planétaire](/assets/images/tutos/033Planetaire/venus3D-04.png){: .align-left}
Les planètes sont toutes logées sur des plateaux (de plus en plus grands) qui sont **tous centrés** sur le Soleil. Pour y arriver il a fallu créer des axes qui s’emboîtent les uns dans les autres. L'extérieur d'un axe a un profil hexagonal afin d'y fixer le plateau d'une planète, et l'intérieur est un creux circulaire pour pouvoir y insérer l'axe de la planète immédiatement plus proche du soleil. Chaque nouvelle planète a donc un axe plus gros (les rayons croissent de +2mm à chaque fois) par rapport à l'axe qu'il va "engloutir", comme des poupées russes. Seul l'axe du Soleil est plein, il mesure 3.8mm de rayon. Celui de Mercure mesure 5.8mm de rayon avec un creux intérieur circulaire de 4mm. Enfin celui de Vénus a un rayon de 7.8mm avec un creux intérieur circulaire de 6mm de rayon. Vous l'aurez compris, celui de la Terre va mesurer 9.8mm de rayon avec un creux circulaire de 8mm de rayon etc .... Enfin pour faciliter la rotation des axes j'ai ajouté un filet de 1mm sur toutes les arrêtes verticales, sinon la rotation n'est pas assez fluide (à noter que j'imprime tout avec une définition de 0.2mm).
{: .text-justify}
A ce stade je ne partage pas encore les STL car ma conception en mode agile où chaque sprint est représenté par une planète me fait modifier un peu certains plateau, notamment le pied et le dernier, ainsi que la taille des axes. Voyez chaque article comme l'aboutissement d'un sprint, en mode démo donc.
{: .text-justify}
{% include figure image_path="/assets/images/tutos/033Planetaire/venus-montage1.jpg" caption="mécanismes avec axes emboités les uns dans les autres" %}
{% include figure image_path="/assets/images/tutos/033Planetaire/venus-montage2.jpg" caption="impression 3D en PLA définition 0.2mm: les rotations des mécaniques sont bien fluides." %}

> Je vais réfléchir au mécanisme de motorisation, avec un moteur pas à pas capable de simuler une journée. Avec un moteur de 200 pas (très courant) il me suffit de définir un nombre de pas entiers pour chaque journée de telle sorte que la sommes des 7 jours fasse pile 200 pas: certains jours seront un peu plus long que d'autres car 200/7 ne donne pas un résultat entier mais chuuuut on ne dira rien à personne, et m'est idée que les week-end seront plus long que les semaines, c'est bien mérité après tout vu que je travaille sur ce projet uniquement sur mon temps libre ....
{: .text-justify}