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
  teaser : "/assets/images/tutos/038SoleilTerreLune/header.jpeg"

#SEO tags
title       : "Planétaire: Soleil Terre Lune"
image       : "/assets/images/tutos/038SoleilTerreLune/header.jpeg"
description : "Cet article détaille l'assemblage d'un planétaire mécanique du système Soleil Terre Lune, entièrement imprimé 3D, avec en option la motorisation du système"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Planétaire"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi"
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["planétaire" ]

---

![Planétaire](/assets/images/tutos/038SoleilTerreLune/header.jpeg){: .align-left}
Cet article détaille l'assemblage de la maquette d'un nouveau [#planétaire](https://papsdroidfr.github.io/tags/#planétaire) mécanique entièrement imprimé 3D. Il est composé du système **(Soleil, Terre, Lune)** animé, avec en option la motorisation du système. Les périodes de révolution et de rotation sont respectées, ainsi que l'axe d'inclinaison de la Terre, ce qui permet d'expliquer visuellement les saisons. La maquette est parfaitement fonctionnelle sans le moteur grâce à un volant.
{: .text-justify}

## Consignes

J'encourage à imprimer les éléments d'une seule étape et de les assembler avant de passer à l'étape suivante. Si vous imprimez tout d'un coup, vous allez galérer à retourver les bonnes roues dentées ou les bons axes de chaque étape. Si vous orientez correctement les pièces lors de l'impression (surface plate au sol) vous n'aurez **pas besoin de support**. Vous pouvez imprimer en **définition 0.3mm** pour gagner du temps, c'est largement suffisant car il n'y a pas de détail fin vertical (comme sur une figurine) qui nécessite d'imprimer en plus petites couches.
{: .text-justify}
Attention certaines pièces font plus que 20cm de long (notemment les supports): cette maquette nécessite une surface d'impression d'au **minimum 25cm x 25cm**.
{: .text-justify}
Chaque fichier STL commence par le numéro de l'étape afin de vous orienter dans le bon ordre d'impression. Les couleurs sont sugérées dans chaque image: pieds en orange, manivelle et tous les axes et roues qui tournent à la vitesse de 1 jour par tour en rouge, autres roues en blanc, axes en noir, supports en gris foncé. Soleil et son axe en jaune, Terre en bleu, Lune en gris clair. Une petite astuce concernant le plateau circulaire pour imprimer les lettrages en blanc sur fond gris foncé sera expliquée lors de cette étape.
{: .text-justify}

## Etapes

### 01: pieds

![Planétaire](/assets/images/tutos/038SoleilTerreLune/01_pieds.png){: .align-center}
Commencez par imprimer (couleur suggérée: orange) tous les pieds de l'étape 01. le pied_exterieur(x4) est à imprimer 4 fois. Le pied_interieur(x3) est à imprimer 3 fois ... vous aurez compris le principe pour la suite: j'indique à chaque fois s'il faut imprimer la même pièce plusieurs fois. Disposez ensuite les pieds à peu près comme sur la photo.
{: .text-justify}

### 02: Plateau circulaire

![Planétaire](/assets/images/tutos/038SoleilTerreLune/02_plateau_circulaire.png){: .align-center}
Pour imprimer les 4 éléments circulaires de la base, j'ai utilisé 2 couleurs en commençant par du blanc sur les couches inférieures, puis du gris foncés sur les couches supérieures de manières à ce que les lettrages apparaissent en blanc sur fond gris foncé. Si vous optez en une définition de **couche de 0.3mm**, il faut changer de bobine au début de la **8ème couche** pour obtenir cet effet. le socle_base quand à lui s'imprime en une seule couleur (gris foncé).
{: .text-justify}

Assemblez les éléments comme sur la photo. N'hésitez pas à **ébarber avec un cuter les surplus de matière** qui empècheraient d'assembler les 1/4 de cercle entre eux ainsi qu'au socle_base: c'est fait pour être serré, calculé au 1/10ème de mm près. Pour assembler le pied qui va maintenir le moteur et la manivelle, ne pas hésiter à **tapper avec un petit marteau**: ça rentre de justesse, c'est fait exprès.
{: .text-justify}

**Optionnel**: pour améliorer l'accroche de la maquette au sol, j'ai rajouté sous chaque pied des petits pâtins que l'on met habituellement sous les pieds de chaises pour éviter de cogner les parquets. Si vous utilisez un moteur: ça limitera les vibrations.
{: .text-justify}

### 03: Manivelle

{% include figure image_path="/assets/images/tutos/038SoleilTerreLune/03_manivelle_assemblage.png" caption="Assemblage de la manivelle sur les pieds du socle" %}

![Planétaire](/assets/images/tutos/038SoleilTerreLune/03_manivelle_300.png){: .align-left}
Les éléments de la manivelle sont imprimés en rouge. Pensez à bien **orienter surface plate au sol** la roue dentée et le volan (attention ils ont tous les deux une colerettes sur une face). Je conseille d'imprimer tous **les axes à la verticale** (pas besoin d'utiliser de bordure ni de radeau pour augmenter la surface d'accroche). Si vous les imprimez à l'horizontale, vous allez subir une petite déformation de la base au sol, et l'axe ne va pas bien tourner dans son logement après.
{: .text-justify}

> Mettez de côté tout cet ensemble pieds, plateau circulaire et manivelle. Vous aller dans les sections suivantes assembler tout le mécanisme du planétaire à part, et le poser sur le plateau circulaire en toute dernière étape.

### 04: Base

{% include figure image_path="/assets/images/tutos/038SoleilTerreLune/04_base_assemblage.png" caption="Assemblage des éléments de la base" %}

![Planétaire](/assets/images/tutos/038SoleilTerreLune/04_base_300.png){: .align-left}
Les axes, roues dentées et colerettes sont à imprimer en rouge (toujours surface plate au sol, axe et colerettes à la verticale). J'ai imprimé les base_support_bas et haut en gris foncé. Pensez à bien réorienter la pièce base_support_haut dans l'autre sens: surface plate au sol et pas besoin de support dans cette configuration. Suivez la photo du haut pour assembler les pièces. Ne vous trompez pas entre les deux roues de 28 dents l'une est à disposer sur l'axe horizontal, l'autre sur l'axe vertical (elles sont différentes). N'oubliez pas les colerettes. Le mécanisme doit être fluide une fois que tout est assemblé.
{: .text-justify}

### 05: Soleil

### 06: Révolution Terre

### 07: Rotation Terre

### 08: Inclinaison Terre

### 09: Révolution Lune

### 10: Lune

### 11: Terre