---
#source: https://www.papsdroid.fr/post/planetaire-engrenages
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
  teaser : "/assets/images/tutos/033Planetaire/02engrenages.jpg"

#SEO tags
title       : "Planétaire: théorie des engrenages"
image       : "/assets/images/tutos/033Planetaire/02engrenages.jpg"
description : "Un peu de théorie (passionnante) avec les engrenages."

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Engrenages"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["planétaire" ]

---
![Planétaire](/assets/images/tutos/033Planetaire/02engrenages.jpg){: .align-left}
Un peu de théorie (passionnante) avec les [engrenages](https://fr.m.wikipedia.org/wiki/Engrenage). Nous avons vu dans [l'article précédent](https://papsdroidfr.github.io/tutoriels/planetaire/) que les orbites dans notre [#planétaire](https://papsdroidfr.github.io/tags/#planétaire) seront représentées par des cercles: nous allons utiliser une belle quantité d'engrenages pour animer notre planétaire mécanique, et il va falloir les disposer de façon à obtenir les **rapports de réduction** les plus fidèles possibles par rapport à la réalité. Si vous n'aimez pas les mathématiques cet article ne va pas vous plaire: c'est dit!
{: .text-justify}

## train d'engrenage simple
![Planétaire](/assets/images/tutos/033Planetaire/02_2roues.png){: .align-left}
Si on considère cette simple petite machine à base de deux roues de 15 et 37 dents, elle offre un rapport de réduction **R = 15/37 = 0.4054 = 1/2,4667**. La notation inverse 1/x est pratique car elle me permet de dire que la grande roue entraînée par la petite va tourner 2,4667 fois plus lentement (inversement, la petite roue tourne 2,4667 fois plus vite que la grande). Une autre particularité à ne pas oublier est que les roues dans cette configuration vont tourner en sens inverse l'une par rapport à l'autre. Le vrai rapport de réduction est en fait **R = (-1)*15/37**.
{: .text-justify}
Si on insère une 3ème roue dentée de n dents entre les deux roues de 15 et 37 dents, les roues vont propager les unes aux autres leurs rapports de réduction et on va obtenir un rapport **R = (-1)*15/n * (-1)*n/37 = (-1)²*(15*n)/(37*n) = 15/37**. On remarque tout de suite que peut importe le nombre de dents de la roue intermédiaire: son seul rôle est d'inverser le sens de rotation. Au final la grande roue tourne toujours 2.4667 fois pus lentement que le petite, mais dans le même sens. 
{: .text-justify}
>On calcule donc le rapport d'un train d'engrenages simple en faisant le rapport du nombre de dents entre le premier et le dernier. Pour savoir s'ils tournent dans le même sens, il faut compter les contacts extérieurs entre roues: ils tourneront dans le même sens si le nombre de contacts extérieurs est pair. A priori ça ne sert à rien d'avoir des trains de plus de 3 roues puisqu'on obtiendra le même résultat avec un train de 2 ou 3 roues. Si les contacts sont "intérieurs" (une roue à l'intérieur d'une autre roue) elles vont tourner dans le même sens.
{: .text-justify}

Prenons comme échelle de temps le jour pour notre planétaire mécanique. La terre tourne autour du soleil en environ 365,25 jours: elle tourne donc 365,25 fois plus lentement autour du soleil que sur elle même. Si je veux réaliser un train simple d'engrenage avec un tel rapport de réduction 1/365.25, rien de plus facile me direz vous: **1/365,25 = 4/1461**. Il va être compliqué de faire une roue dentée avec 4 dents, le minimum étant 13 on va multiplier tout ça par 4 pour obtenir **1/365.25 = 16/5844**. Il faut partir d'une petite roue de 16 dents et utiliser une grosse de 5844 dents.... sauf que ça ne va pas être possible du tout ! La circonférence de la roue est proportionnelle à son nombre de dents, donc la roue de 5844 dents aura une circonférence grosso-modo 365 fois plus longue que la petite roue de 16 dents. La circonférence est aussi proportionnelle au diamètre de la roue dentée (c = pi*d), donc dit autrement la roue de 5844 dents est 365 fois plus grande que la petite de 16 dents. Si la petite mesure 1,5 cm de diamètre , et bien la grande va mesurer ... **5m47 !!** autant dire qu'on va oublier l'idée.
{: .text-justify}
>Puisqu'on limite le nombre de dents (moins de 100) pour ne pas se retrouver avec des engrenages gigantesques, les trains simple d'engrenages ne permettent pas d'obtenir de gros rapports de réduction. Ils sont aussi limités en précision puisque **R = (-1)^n * d1/d2**.
{: .text-justify}
Bref pour obtenir les très gros rapports de réduction du système solaire (Saturne tourne autour du soleil en 30 ans = 10950 jours il va falloir ruser !
{: .text-justify}

## Train d'engrenages à étage
![Planétaire](/assets/images/tutos/033Planetaire/02_4roues.png){: .align-left}
Considérons maintenant cette machine composée d'une paire d'engrenages de 15 et 37 dents **sur 2 étages**. Le premier étage c'est notre machine précédente composée d'un engrenage de deux roues 15 et 37 dents qui offre un rapport de (-1)15/37 = -1/2,4667. La deuxième petite roue grise de 15 dents est fixée sur le même axe que la grosse roue de 37 dents en dessous: elle est montée en étage de la grosse roue et va tourner exactement à la même vitesse. A son tour cette petite roue entraîne la deuxième roue de 37 dents sur son étage avec un rapport de -1/2,4667. Mais comme la petite roue de cet étage tourne déjà 2.4667 fois plus lentement que la première petite roue: au final le rapport de réduction entre la petite du 1er étage et la grosse roue du second est de: (-1/2,4667)² = 1/6,0846. Et on peut continuer à jouer comme ça longtemps...: en remettant encore un étage au dessus de la deuxième roue de 37 dents, on obtiendrait un rapport de **(-1/2.4667)³ = -1/15** ! Bref on parvient à démultiplier les rapports de réduction grâce à un montage de train d'engrenages à étages.
{: .text-justify}
Dans un train d'engrenages à étage on distingue les roues menantes Z des roues menées D. Le rapport de réduction se calcule par la formule:
{: .text-justify}

```
R = (-1)^y * (Z1*Z2*..*Zn) / (D1*D2*...Dm)
```

* y est le nombre de contacts extérieurs entre les dents.
* Zi, Dj le nombre de dents d'une roue.
* n le nombre de roue menantes.
* m le nombre de roue menées.

>A partir d'une liste d'engrenages bien définis entre 13 et une centaines de dents, trouver un rapport de réduction R revient à trouver la bonne combinaison d'engrenages qui permet de **résoudre cette équation**, qui peut s'avérer être un vrai casse-tête si on essaye d'y aller à tâtons. Nous verrons dans le prochain chapitre comment la résoudre avec l'aide d'une indispensable simulation en Python.
{: .text-justify}
