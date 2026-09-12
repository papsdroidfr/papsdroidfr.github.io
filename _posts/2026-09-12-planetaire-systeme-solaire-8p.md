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

> Tous les fichiers STL sont à disposition sur mon profil [cults3D](https://cults3d.com/fr/mod%C3%A8le-3d/art/planetaire-soleil-terre-lune)
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
Imprimez la **collerette de montage 5mm**: elle va servir pour bien positionner les roues dentées sur leur axe.
{: .text-justify}
Le petit axe **mercure_axe_motor** se positionne sur l'axe métallique du moteur. Il y a un méplat sur cet axe: **une encoche est visible** en haut de l'axe pour **matérialiser le méplat**. Elle vous permet de bien positionner l'axe pour l'enfoncer (à l'aide d'un petit marteau) sur l'axe métallique du moteur. Positionnez ensuite par dessus le second axe plus large qui va entraîner l'engrenage: **mercure_axe_gear_motor**. Ce principe sera identique avec les 7 autres moteurs.
{: .text-justify}
Le **support** des engrenages se positionne ensuite par dessus le support du Soleil. Il faut ensuite positionner l'axe central par dessus l'axe du soleil.
{: .text-justify}
Viennent ensuite les **3 roues dentées** (la roue de 23 dents est à imprimer 2 fois): une première roue de 23 dents sur l'axe moteur, puis au milieu sur un petit axe une roue de 24 dents (elle se différencie des roues de 23 dents grâce aux deux petites marques ronde incrustées), et enfin la seconde roue de 23 dents sur l'axe central.
{: .text-justify}
>**Astuce**: utilisez la **collerette de montage 5mm** pour positionner une roue sur son axe avant de la poser sur son support. Un **petit marteau** est nécessaire pour bien enfoncer la roue sur son axe, le jeu est volontairement réduit au minimum pour que la roue soit bien solidaire de son axe. Vous aurez du mal à l'enfoncer à mains nues c'est normal.
{: .text-justify}

### Vénus

### Terre

### Mars

### Jupiter

### Saturne

### Uranus

### Neptune

### Planètes

## Électronique de commande
