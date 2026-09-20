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
  teaser : "/assets/images/tutos/045SystSolaire/syst_solaire_300.png"

#SEO tags
title       : "Planétaire: système solaire"
image       : "/assets/images/tutos/045SystSolaire/syst_solaire_300.png"
description : "Cet article détaille l'assemblage d'un planétaire mécanique du système solaire avec ses 8 planètes."

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Système Solaire"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi"
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["planétaire"]

gallery_uranus:
  - url: /assets/images/tutos/045SystSolaire/09_engrenage_uranus_01_640.png
    image_path: "/assets/images/tutos/045SystSolaire/09_engrenage_uranus_01_300.png"
    title: "premier support engrenages"
  - url: /assets/images/tutos/045SystSolaire/09_engrenage_uranus_02_640.png
    image_path: "/assets/images/tutos/045SystSolaire/09_engrenage_uranus_02_300.png"
    title: "engrenage sur le premier support"
  - url: /assets/images/tutos/045SystSolaire/09_engrenage_uranus_03_640.png
    image_path: "/assets/images/tutos/045SystSolaire/09_engrenage_uranus_03_300.png"
    title: "second support engrenage"
---

![Planétaire](/assets/images/tutos/045SystSolaire/syst_solaire_300.png){: .align-left}
Notice de montage d'un nouveau [#planétaire](https://papsdroidfr.github.io/tags/#planétaire) représentant le système solaire 100% imprimé 3D, avec les 8 planètes **(Mercure, Vénus, Terre, Mars, Jupiter, Saturne, Uranus, Neptune)**, entraînés par un système motorisé qui synchronise toutes les révolutions des planètes.  
{: .text-justify}

## Matériel nécessaire

**Une imprimante 3D** avec un plateau de dimension minimum **30cmx30cm** minimum."
{: .text-justify}
**Un petit marteau**: certaines pièces sont difficiles à assembler à main nues, c'est fait exprès pour éviter qu'il n'y ait trop de jeu.
{: .text-justify}
**Plusieurs bobines de PLA/PLA+** (la maquette pèse presque 4kg en tout), j'ai utilisé comme couleurs:
{: .text-justify}
- silk noir pour les supports et axes
- blanc pour les roues dentées.
- jaune vif pour le soleil et son axe
- gris pour Mercure
- marron pour Vénus
- bleu vif pour la Terre
- rouge pour Mars
- brun clair pour Jupiter
- brun foncé pour Saturne
- bleu ciel pour Uranus
- bleu clair pour Neptune
  
**De la colle PVC** pour assembler quelques pièces (les planètes notamment).
{: .text-justify}

**Attention** ce planétaire ne fonctionne qu'avec le système de motorisation avec **8 moteurs** pas à pas **NEMA17** accompagné du [système de contrôle de 8 moteurs](https://papsdroidfr.github.io/tutoriels/16bitsParallelControlCard/). Vous pouvez me contacter si vous voulez récupérer une carte PCB j'en ai fait faire en  plusieurs exemplaires, mais il va falloir jouer du fer à souder.
{: .text-justify}

## Consignes

> Tous les fichiers STL sont à disposition sur mon profil [cults3D](UPDATE https://cults3d.com/fr/mod%C3%A8le-3d/art/planetaire-soleil-terre-lune)
{: .text-justify}

J'encourage à imprimer les éléments d'une seule étape et de les assembler avant de passer à l'étape suivante. Si vous imprimez tout d'un coup, vous allez galérer à retrouver les bonnes roues dentées ou les bons axes de chaque étape. Si vous orientez correctement les pièces lors de l'impression (surface plate au sol) vous n'aurez **pas besoin de support**. Vous pouvez imprimer en **définition 0.3mm** pour gagner du temps, c'est largement suffisant car il n'y a pas de détail fin vertical (comme sur une figurine) qui nécessite d'imprimer en plus petites couches, sauf **les planètes** sphériques qu'il faut imprimer en **0,15mm** pour un meilleur rendu. Enfin je conseille d'imprimer **les axes en position verticale**, sinon l'impression va légèrement écraser un bord.
{: .text-justify}
Attention certaines pièces font plus que 25cm de long (notamment certains supports): cette maquette nécessite une surface d'impression d'au **minimum 30cm x 30cm**.
{: .text-justify}
Chaque fichier STL commence par le nom d'une planète pour vous orienter dans le bon ordre d'impression. Les couleurs sont suggérées dans chaque image. Si un fichier stl a un (x) à la fin c'est qu'il faut l'imprimer x fois. Par exemple mercure_gear24(x2).stl signifie que cette roue dentée de 24 dents doit être imprimée 2 fois.
{: .text-justify}

> **Bon print et bon assemblage !**

## Étapes impression 3D

### Soleil
![Planétaire](/assets/images/tutos/045SystSolaire/01_soleil.png){: .align-left}
Cette première étape est simple: imprimez: 
* le pied (silk noir), 
* les deux axes (jaune vif, en position verticale), 
* l'axe d'assemblage (il sert à assembler les 2 demis-sphères)
* les 2 demi-sphères du soleil (jaune vif, hauteur de couche 0.15mmm).

L'assemblage est simple mais laissez le soleil de côté ce sera la dernière pièce de la maquette à poser.
{: .text-justify}

### Support moteurs
![Planétaire](/assets/images/tutos/045SystSolaire/02_base_moteurs.png){: .align-center}
Il faut imprimer 8 fois le support bas et les assembler avec l'aide d'un petit marteau. Positionnez ensuite les moteurs NEMA17 dans chaque emplacement avec le **branchement de câble vers l'extérieur**. Imprimez 8 fois les supports haut, assemblez-les avec un petit marteau et postionnez les par dessus les moteurs.
{: .text-justify}

### Mercure
![Planétaire](/assets/images/tutos/045SystSolaire/03_engrenage_mercure..png){: .align-center}
Imprimer la **collerette de montage 5mm**: elle va servir pour bien positionner les roues dentées sur leur axe.
{: .text-justify}
Le petit axe **mercure_axe_motor** se positionne sur l'axe métallique du moteur. Il y a un méplat sur cet axe: **une encoche est visible** en haut de l'axe pour **matérialiser le méplat**. Elle vous permet de bien positionner l'axe pour l'enfoncer (à l'aide d'un petit marteau) sur l'axe métallique du moteur. Positionnez ensuite par dessus le second axe plus large qui va entraîner l'engrenage: **mercure_axe_gear_motor**. Ce principe sera identique avec les 7 autres moteurs.
{: .text-justify}
Le **support** des engrenages se positionne ensuite par dessus le support du Soleil. Il faut ensuite positionner l'axe central par dessus l'axe du soleil.
{: .text-justify}
Viennent ensuite les **3 roues dentées** (la roue de 24 dents est à imprimer 2 fois): une première roue de 24 dents sur l'axe moteur, puis au milieu sur un petit axe une roue de 23 dents (elle se différencie des roues de 24 dents grâce aux deux petites marques ronde incrustées), et enfin la seconde roue de 24 dents sur l'axe central. Sur chaque roue il y a une **fine collerette de 1mm** autour du passage de l'axe: identifiez la avec vos ongle: **elle doit toujours se situer au dessus**. Son rôle et de limiter les frottements de la roue avec les supports.
{: .text-justify}
>**Astuce**: utilisez la **collerette de montage 5mm** pour positionner une roue sur son axe avant de la poser sur son support. Un **petit marteau** est nécessaire pour bien enfoncer la roue sur son axe, le jeu est volontairement réduit au minimum pour que la roue soit bien solidaire de son axe. Vous aurez du mal à l'enfoncer à mains nues c'est normal.
{: .text-justify}

### Vénus
![Planétaire](/assets/images/tutos/045SystSolaire/04_engrenage_venus.png){: .align-center}
Chaque étape commence avec l'axe_moteur à positionner sur l'axe métallique du moteur (bien orienter le méplat), puis l'axe_gear_moteur à mettre par dessus et la collerette moteur par-dessus. Positionnez le support_engrenage,  et vous pouvez alors poser l'axe_central par dessus l'axe de la planète précédente (Mercure).
{: .text-justify}
**L'engrenage est composé de 4 roues** (2x17, 1x18 et 1x41 dents) sur 2 étages. Positionner la roue gear17_001 avec sa grande collerette sur l'axe qui dépasse du moteur: il faut pousser fort, utiliser un marteau en tapotant délicatement pour qu'il rentre sur son axe. Au milieu il faut positioner sur l'axe_interne la roue gear18 (en dessous) et la gear17_002 par dessus. Utiliser la collerette de montage 5mm pour bien positioner ces 2 roues sur leur axe avant de le placer sur le support (**la roue de 18 dents doit être en dessous**). Placer ensuite la plus grosse roue gear41 sur l'axe central jusqu'à ce quelle vienne en butée du support. Rappelez-vous bien que les fines collerettes sur les roues doivent être placées en haut.
{: .text-justify}

>**Astuce**: positionner la grande roue par dessous l'axe central au lieu d'essayer de la faire descendre tout l'axe, et ajuster jusqu'à ce que la roue vienne en butée du support.
{: .text-justify}

### Terre
![Planétaire](/assets/images/tutos/045SystSolaire/05_engrenage_terre.png){: .align-center}
Comme à chaque étape, commencer par positionner l'axe_moteur, l'axe_gear_moteur, et la collerette_moteur sur l'axe métallique du moteur. Ensuite positionner le support_engrenage et l'axe_central par dessus l'axe central de la précédente planète (Vénus).
{: .text-justify}

**L'engrenage est composé de 5 roues** (1x15, 2x17, 1x20 et 1x30 dents). La roue gear17_001 avec la grande collerette se positionne sur l'axe qui dépasse du moteur. Vient ensuite déportée sur la gauche une petite roue gear15 sur un axe_interne. Ensuite sur un second axe_interne, positionner 2 roues gear20 (par dessous) et gear17_002 (par dessus). Enfin la grande roue gear30 sur l'axe central jusqu'en butée du support.
{: .text-justify}

>**Rappel** la **collerette de montage 5mm** est votre amie pour bien positionner les roues sur leur axe interne, avec l'aide d'un petit marteau en tapotant doucement.
{: .text-justify}

### Mars
![Planétaire](/assets/images/tutos/045SystSolaire/06_engrenage_mars.png){: .align-center}
Positionner l'axe_moteur, l'axe_gear_moteur, et la collerette_moteur sur l'axe métallique du moteur. Ensuite positionner le support_engrenage et l'axe_central par dessus l'axe central de la précédente planète (Terre).
{: .text-justify}

**L'engrenage est composé de 4 roues** (1x16, 1x17, 1x18 et 1x59) sur 2 étages. La roue à poser sur l'axe moteur est la roue de 16 dents. La collerette_gear16 est à poser par dessus cette roue. A côté viennent dans l'ordre deux roues de 18 (dessous) et 17 (dessus) sur le même axe_interne. Bien orienter les fines collerettes sur le dessus comme à chaque fois. La grosse roue de 59 dents se positionne sur l'axe_central, collerette orientée au dessus.
{: .text-justify}

### Jupiter
![Planétaire](/assets/images/tutos/045SystSolaire/07_engrenage_jupiter.png){: .align-center}
On répète encore les mêmes gestes au début mais avec des pièces de plus en plus grandes à imprimer: positionner l'axe_moteur, l'axe_gear_moteur, et la collerette_moteur sur l'axe métallique du moteur. Ensuite positionner le support_engrenage et l'axe_central par dessus l'axe central de la précédente planète (Mars).
{: .text-justify}

**L'engrenage est composé de 7 roues** (2x15, 1x17, 2x28, 2x29) sur 3 étages. Sur l'axe du moteur se positionne l'une des deux roues gear15 et la collerette_13mm. Devant sur un axe_interne: une roue gear_28 (dessous) et une roue gear15 ainsi que la collerette_7mm. A l'équerre on retrouve sur un axe_interne une roue gear29 (dessous) avec une roue gear17(dessus) avec la fine collerette intégrée positionnée sur le dessus. Ensuite près de l'axe central il y a la seconde roue gear28 sur le troisième axe_interne. Pour finir la dernière gear29_002 se positionne sur l'axe central, en butée sur le support.
{: .text-justify}

### Saturne
![Planétaire](/assets/images/tutos/045SystSolaire/08_engrenage_saturne_02.png){: .align-center}

![Planétaire](/assets/images/tutos/045SystSolaire/08_engrenage_saturne_01.png){: .align-left}
Ca se complique un tout petit peu pour Saturne car le support de l'engrenage se compose de deux pièces: positionner comme d'habitude l'axe_moteur, l'axe_gear_moteur, et la collerette_moteur sur l'axe métallique du moteur. Ensuite positionner les deux support_engrenage, et l'axe_central par dessus l'axe central de la précédente planète (Jupiter).
{: .text-justify}

**L'engrenage est composé de 6 roues** (1x13, 2x16, 1x34, 1x41, 1x73) sur 3 étages. La roue gear13 est à positionner sur l'axe du moteur, avec la collerette_15mm par dessus. Ensuite sur un axe_interne il y a un étage avec une roue gear34 (dessous) et une roue gear16 (celle qui n'a pas de petite collerette), avec la collerette_9mm à positionner par dessus. Sur un second axe_interne, il y a une roue gear41 (dessous) par dessus laquelle se positionne la seconde gear16 qui a une petite collerette intégrée (à orienter au dessus). Enfin la grosse roue gear73 est à positionner sur l'axe_central en butée contre son support. La collerette_montage est utile pour bien la positionner sur son axe.
{: .text-justify}

### Uranus

![Planétaire](/assets/images/tutos/045SystSolaire/09_engrenage_uranus.png){: .align-left}
Une nouvelle petite complexité avec Uranus: le support_engrenage est en deux parties tout comme avec Saturne, mais **ils se positionnent sur 2 étages différents**: positionner comme d'habitude l'axe_moteur, l'axe_gear_moteur, et la collerette_moteur sur l'axe métallique du moteur. Ensuite positionner le premier support_engrenage (la plus grande pièce), et l'axe_central par dessus l'axe central de la précédente planète (Saturne).
{: .text-justify}

{% include gallery id="gallery_uranus" caption="Cliquez pour agrandir les images" %}

**L'engrenage est composé de 6 roues** (3x13, 1x23, 1x49, 1x83) sur 3 étages. La roue gear13_001 est à positionner sur l'axe du moteur, avec au dessus la collerette_7mm. Ensuite sur l'axe_interne_001 la roue gear23 (en dessous) avec au dessus la seconde roue gear13_002 qui a déjà sa propre fine collerette à orienter au dessus. Viennent ensuite sur le seconde axe_interne_002 une roue gear49 (en dessous) puis au dessus la troisième roue gear13_003 qui est plus épaisse que les autres, avec sa fine collerette qu'il faut orienter au dessus.
{: .text-justify}

Positionner à ce moment le second support_engrenage_002. Il faut alors finir par poser la grosse roue gear85, en butée sur son support.
{: .text-justify}


### Neptune

![Planétaire](/assets/images/tutos/045SystSolaire/13_engrenage_neptune.png){: .align-left}
Concernant Neptune, le support_engrenage est aussi en deux parties tout comme avec Saturne et Uranus, et **ils se positionnent sur 2 étages différents** comme avec Uranus. Positionner comme d'habitude l'axe_moteur, l'axe_gear_moteur, et la collerette_moteur sur l'axe métallique du moteur.
{: .text-justify}

![Planétaire](/assets/images/tutos/045SystSolaire/14_engrenage_neptune_04.png){: .align-left}
Mais regardez bien le haut de la collerette_moteur: il y a un petit passage pour que la grosse roue de 83 dents ne viennent pas s'y accrocher. **Il est important d'utiliser un petit point de colle PVC sur le bas** pour que cette collerette ne tourne pas avec l'axe du moteur. Attention à ne pas mettre trop de colle il ne faut pas que ça déborde sur l'axe. **Conseil**: ne mettez la colle qu'à la fin lorsque le moteur est prêt à tourner,pour être certain de ne pas encoller l'axe.
{: .text-justify}

Ensuite positionner le premier support_engrenage (la plus grande pièce), et l'axe_central par dessus l'axe central de la précédente planète (uranus).
{: .text-justify}

### Planètes

## Électronique de commande
