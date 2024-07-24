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

gallery_base:
  - url: /assets/images/tutos/038SoleilTerreLune/04_base_640_1.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/04_base_300_1.png"
    title: "Assemblage de la base"
  - url: /assets/images/tutos/038SoleilTerreLune/04_base_640_2.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/04_base_300_2.png"
    title: "Montage final"

gallery_soleil:
  - url: /assets/images/tutos/038SoleilTerreLune/05_soleil_axe_assemblage_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/05_soleil_axe_assemblage.png"
  - url: /assets/images/tutos/038SoleilTerreLune/05_soleil_axe_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/05_soleil_axe.png"

gallery_revol_terre:
  - url: /assets/images/tutos/038SoleilTerreLune/06_revolution_terre_02_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/06_revolution_terre_02_300.png"
  - url: /assets/images/tutos/038SoleilTerreLune/06_revolution_terre_03_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/06_revolution_terre_03_300.png"
  - url: /assets/images/tutos/038SoleilTerreLune/06_revolution_terre_04_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/06_revolution_terre_04_300.png"

gallery_rotation_terre_plateau:
  - url: /assets/images/tutos/038SoleilTerreLune/07_rotation_terre_plateau_assemblage_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/07_rotation_terre_plateau_assemblage_300.png"
  - url: /assets/images/tutos/038SoleilTerreLune/07_rotation_terre_plateau_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/07_rotation_terre_plateau_300.png"

gallery_inclinaison_terre:
  - url: /assets/images/tutos/038SoleilTerreLune/08_inclinaison_terre_01_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/08_inclinaison_terre_01_300.png"
  - url: /assets/images/tutos/038SoleilTerreLune/08_inclinaison_terre_02_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/08_inclinaison_terre_02_300.png"
  - url: /assets/images/tutos/038SoleilTerreLune/08_inclinaison_terre_03_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/08_inclinaison_terre_03_300.png"

gallery_revolution_lune:
  - url: /assets/images/tutos/038SoleilTerreLune/09_revol_lune_01_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/09_revol_lune_01_300.png"
  - url: /assets/images/tutos/038SoleilTerreLune/09_revol_lune_02_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/09_revol_lune_02_300.png"
  - url: /assets/images/tutos/038SoleilTerreLune/09_revol_lune_03_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/09_revol_lune_03_300.png"

gallery_lune:
  - url: /assets/images/tutos/038SoleilTerreLune/10_lune_01_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/10_lune_01_300.png"
  - url: /assets/images/tutos/038SoleilTerreLune/10_lune_02_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/10_lune_02_640.png"

gallery_terre:
  - url: /assets/images/tutos/038SoleilTerreLune/11_terre_01_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/11_terre_01_300.png"
  - url: /assets/images/tutos/038SoleilTerreLune/11_terre_02_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/11_terre_02_300.png"
  - url: /assets/images/tutos/038SoleilTerreLune/11_terre_03_640.png
    image_path: "/assets/images/tutos/038SoleilTerreLune/11_terre_03_640.png"
---

![Planétaire](/assets/images/tutos/038SoleilTerreLune/header.jpeg){: .align-left}
Cet article détaille l'assemblage de la maquette d'un nouveau [#planétaire](https://papsdroidfr.github.io/tags/#planétaire) mécanique entièrement imprimé 3D. Il est composé du système **(Soleil, Terre, Lune)** animé, avec en option la motorisation du système. Les périodes de révolution et de rotation sont respectées, ainsi que l'axe d'inclinaison de la Terre, ce qui permet d'expliquer visuellement les saisons. La maquette est parfaitement fonctionnelle sans le moteur grâce à un volant.
{: .text-justify}

## Consignes

J'encourage à imprimer les éléments d'une seule étape et de les assembler avant de passer à l'étape suivante. Si vous imprimez tout d'un coup, vous allez galérer à retrouver les bonnes roues dentées ou les bons axes de chaque étape. Si vous orientez correctement les pièces lors de l'impression (surface plate au sol) vous n'aurez **pas besoin de support**. Vous pouvez imprimer en **définition 0.3mm** pour gagner du temps, c'est largement suffisant car il n'y a pas de détail fin vertical (comme sur une figurine) qui nécessite d'imprimer en plus petites couches.
{: .text-justify}
Attention certaines pièces font plus que 20cm de long (notamment les supports): cette maquette nécessite une surface d'impression d'au **minimum 25cm x 25cm**.
{: .text-justify}
Chaque fichier STL commence par le numéro de l'étape afin de vous orienter dans le bon ordre d'impression. Les couleurs sont suggérées dans chaque image: pieds en orange, manivelle et tous les axes et roues qui tournent à la vitesse de 1 jour par tour en rouge, autres roues en blanc, axes en noir, supports en gris foncé. Soleil et son axe en jaune, Terre en bleu, Lune en gris clair. Une petite astuce concernant le plateau circulaire pour imprimer les lettrages en blanc sur fond gris foncé sera expliquée lors de cette étape.
{: .text-justify}

## Étapes

### 01: Pieds

![Planétaire](/assets/images/tutos/038SoleilTerreLune/01_pieds.png){: .align-center}
Commencez par imprimer (couleur suggérée: orange) tous les pieds de l'étape 01. le pied_extérieur(x4) est à imprimer 4 fois. Le pied_intérieur(x3) est à imprimer 3 fois ... vous aurez compris le principe pour la suite: j'indique à chaque fois s'il faut imprimer la même pièce plusieurs fois. Disposez ensuite les pieds à peu près comme sur la photo.
{: .text-justify}

### 02: Plateau circulaire

![Planétaire](/assets/images/tutos/038SoleilTerreLune/02_plateau_circulaire.png){: .align-center}
Pour imprimer les 4 éléments circulaires de la base, j'ai utilisé 2 couleurs en commençant par du blanc sur les couches inférieures, puis du gris foncés sur les couches supérieures de manières à ce que les lettrages apparaissent en blanc sur fond gris foncé. Si vous optez en une définition de **couche de 0.3mm**, il faut changer de bobine au début de la **8ème couche** pour obtenir cet effet. le socle_base quand à lui s'imprime en une seule couleur (gris foncé).
{: .text-justify}

Assemblez les éléments comme sur la photo. N'hésitez pas à **ébarber avec un cuter les surplus de matière** qui empêcheraient d'assembler les 1/4 de cercle entre eux ainsi qu'au socle_base: c'est fait pour être serré, calculé au 1/10ème de mm près. Pour assembler le pied qui va maintenir le moteur et la manivelle, ne pas hésiter à **tapper avec un petit marteau**: ça rentre de justesse, c'est fait exprès.
{: .text-justify}

**Optionnel**: pour améliorer l'accroche de la maquette au sol, j'ai rajouté sous chaque pied des petits patins que l'on met habituellement sous les pieds de chaises pour éviter de cogner les parquets. Si vous utilisez un moteur: ça limitera les vibrations.
{: .text-justify}

### 03: Manivelle

{% include figure image_path="/assets/images/tutos/038SoleilTerreLune/03_manivelle_assemblage.png" caption="Assemblage de la manivelle sur les pieds du socle." %}

![Planétaire](/assets/images/tutos/038SoleilTerreLune/03_manivelle_300.png){: .align-left}
Les éléments de la manivelle sont imprimés en rouge. Pensez à bien **orienter surface plate au sol** la roue dentée et le volant (attention ils ont tous les deux une collerettes sur une face). Je conseille d'imprimer tous **les axes à la verticale** (pas besoin d'utiliser de bordure ni de radeau pour augmenter la surface d'accroche). Si vous les imprimez à l'horizontale, vous allez subir une petite déformation de la base au sol, et l'axe ne va pas bien tourner dans son logement après.
{: .text-justify}

> Mettez de côté tout cet ensemble pieds, plateau circulaire et manivelle. Vous aller dans les sections suivantes assembler tout le mécanisme du planétaire à part, et le poser sur le plateau circulaire en toute dernière étape.
{: .text-justify}

### 04: Base

{% include gallery id="gallery_base" caption="Cliquez pour agrandir les images" %}
Les axes, roues dentées et collerettes sont à imprimer en rouge (toujours surface plate au sol, axe et collerettes à la verticale). J'ai imprimé les base_support_bas et haut en gris foncé. Pensez à bien réorienter la pièce base_support_haut dans l'autre sens: surface plate au sol et pas besoin de support dans cette configuration. Suivez la photo de gauche (cliquez dessous pour agrandir) pour voir comment assembler les pièces. Ne vous trompez pas entre les deux roues de 28 dents l'une est à disposer sur l'axe horizontal, l'autre sur l'axe vertical (elles sont différentes). N'oubliez pas les collerettes. Le mécanisme doit être fluide une fois que tout est assemblé.
{: .text-justify}

La petite roue de 28 sur l'axe horizontal est difficile à poser à cause de l'axe hexagonal et le peu d'espace, mais on y arrive en essayant d'enfoncer l'axe tout en le tournant un peu pour qu'il se positionne bien dans le trou hexagonal de la roue.
{: .text-justify}

### 05: Soleil

![Planétaire](/assets/images/tutos/038SoleilTerreLune/05_soleil.png){: .align-left}
Bien entendu pour le soleil vous pouvez sortir une bobine jaune. Tout comme les autres éléments pensez à bien poser la surface plate au sol avant d'imprimer les pièces, et imprimez les axes verticalement pour ne pas les déformer. Assemblez d'abord la partie haute et basse du soleil avec le petit axe de fixation, puis posez ce soleil de côté: vous le **placerez en tout dernier** sur la maquette.
{: .text-justify}

{% include gallery id="gallery_soleil" caption="Cliquez pour agrandir les images." %}
Imprimez ensuite l'axe fixe interne et la petite collerette. La **surface bombée** de cette collerette doit se mettre en **haut**.
{: .text-justify}

### 06: Révolution Terre

![Planétaire](/assets/images/tutos/038SoleilTerreLune/06_revolution_terre_300.png){: .align-left}
C'est parti pour imprimer l'engrenage qui permet de gérer la **révolution de la Terre** en **365,256 jours**. Si vous voulez savoir comment les roues dentées ont été calculées pour obtenir cette précision, tout est expliqué [dans cet article](https://papsdroidfr.github.io/dev/CalculEngrenages/) avec un rappel théorique sur le calcul d'un rapport de réduction. J'ai opté pour imprimer les **axes** en noirs, et les roues dentées en blanc.
{: .text-justify}

#### Engrenage central

![Planétaire](/assets/images/tutos/038SoleilTerreLune/06_revolution_terre_01_640.png){: .align-center}
Commencez par **enlever le support haut de la base**, puis imprimez tous les axes (en noir, à la verticale). L'axe creux de 10mm se positionne sur l'axe central rouge en veillant à bien mettre **la collerette de l'axe vers le haut.** Les 3 autres axes de 5mm ont 3 tailles différentes: chacune va dans son logement approprié (du plus petit au plus haut).
{: .text-justify}

Imprimez ensuite les **roues dentées** en blanc ainsi que la **collerette de 12mm**: 3 roues de 16 dents, 1 roue de 40 dents et 2 roues de 82 dents. Si les roues sont dures à insérer sur les axes mettez un coup de cutter pour débarrasser les surplus de matière gênants. Positionnez les comme sur le dessin:
{: .text-justify}

* **sur le petit axe**: une roue de 40 et la roue de 16 au dessus.
* **sur l'axe moyen**: une roue de 82 et une roue de 16 au dessus.
* **sur le grand axe**: une roue de 82, la collerette de 12mm et une roue de 16 au dessus.

> En tournant l'axe de la manivelle, la rotation des axes doit être fluide, sinon ébarbez avec un cutter ou un petit tournevis plat les surplus de matière qui gênent à l'intérieur des support des axes. Tout est calculé avec un jeux de 0,1mm pour que les axes ne partent pas dans tous les sens quand la rotation s'active.
{: .text-justify}

#### Roue support du plateau

{% include gallery id="gallery_revol_terre" caption="Cliquez pour agrandir les images" %}
Vous pouvez **remettre le support_haut de la base**, et imprimez en blanc la collerette de 1mm ainsi que la grosse roue de 89 dents. N'oubliez surtout pas de positionner **cette collerette de 1mm sous la roue** de 89 dents.
{: .text-justify}

> Testez le mécanisme qui doit être parfaitement fluide lors de la rotation de la manivelle.
{: .text-justify}

### 07: Rotation Terre

![Planétaire](/assets/images/tutos/038SoleilTerreLune/07_rotation_terre_300.png){: .align-left}
L'engrenage qui va gérer la rotation de la Terre sur elle-même va être supporté par un plateau qui va tourner au même rythme que le mécanisme qui gère la révolution de la Terre. Ce plateau va évoluer sur un pied qui va glisser le long du plateau circulaire fabriqué lors des premières étapes.
{: .text-justify}
{% include gallery id="gallery_rotation_terre_plateau" caption="Cliquez pour agrandir les images" %}
Commencez par imprimer **le plateau** ainsi que le **pied** et les **2 roues**. Le plateau vient se fixer sur la grosse roue dentée de 89 dents de l'engrenage de la révolution de la Terre. **Je conseille d'enlever cette roue** pour la fixer sur le plateau en serrant fort (les encoches de la roue doivent être enfoncées au maximum sur le plateau), et de la remettre sur place avec son plateau bien fixé.
{: .text-justify}

![Planétaire](/assets/images/tutos/038SoleilTerreLune/07_rotation_terre_assemblage_640.png){: .align-center}
Vous pouvez ensuite imprimer les **5 axes** et **6 roues dentées** du mécanisme. **Attention** la toute première roue dentée de 29 dents (07_rotation_terre_gear29_axe_soleil) **est différente** des 3 autres roues de 29 dents (les dents n'ont pas le même profil). Ne vous mélangez surtout pas lorsqu'elles sont imprimées: **sa place est sur l'axe central du Soleil** (enlevez-le d'abord pour positionner cette roue). Les 3 petits axes sont identiques. Le grand axe est à reserver en bout de plateau pour l'axe de rotation de la Terre. Quand à l'axe moyen, il doit se positionner sur la 3ème roue comme sur la photo. Il va communiquer la rotation 1jour/tour au mécanisme de révolution de la Lune autour de la Terre.
{: .text-justify}

> Testez bien le mécanisme avant de passer à l'étape suivante: il doit être parfaitement fluide lors de la rotation de la manivelle. Si les roues du plateau ont tendance à sortir toutes seules de leur logement ce n'est pas grave: elles seront guidées par le plateau circulaire plus tard.
{: .text-justify}

### 08: Inclinaison Terre

![Planétaire](/assets/images/tutos/038SoleilTerreLune/08_inclinaison_terre_300.png){: .align-left}
Ce nouveau **plateau** avec ses **3 roues dentées** et **2 axes** vont permettre de gérer l'engrenage qui maintient l'axe incliné de la Terre dans la même position tout au long de sa révolution autour du Soleil. Pour cela notez bien que la roue de 92 dents doit être fixe sur l'axe sur soleil qui est fixe: c'est curieux mais le rôle de cette roue dentée est bien **de ne pas tourner**.
{: .text-justify}
{% include gallery id="gallery_inclinaison_terre" caption="Cliquez pour agrandir les images" %}
Positionnez d'abord le plateau et **enfoncez bien les petits renforts** qui servent à maintenir les plateaux ensembles. Imprimez ensuite les 2 axes (à la verticale) pour finir avec les 3 roues de 92 dents, 90 (au centre) et 92 à nouveau sur l'axe de la Terre. Les collerettes présentes sur les roues doivent être orientées vers le haut. Le montage suivant devrait ressembler à l'image ci-dessous.
{: .text-justify}
![Planétaire](/assets/images/tutos/038SoleilTerreLune/08_inclinaison_terre_640.png){: .align-center}

### 09: Révolution Lune

![Planétaire](/assets/images/tutos/038SoleilTerreLune/09_revol_lune_300.png){: .align-left}
Nous enchaînons avec ce dernier plateau qui regroupe les 4 axes et 6 roues dentées qui vont gérer la **révolution de la Lune** en 27,32 jours autour de la Terre.
{: .text-justify}
![Planétaire](/assets/images/tutos/038SoleilTerreLune/09_revol_lune_01_640.png){: .align-center}
![Planétaire](/assets/images/tutos/038SoleilTerreLune/09_revol_lune_02_640.png){: .align-center}
![Planétaire](/assets/images/tutos/038SoleilTerreLune/09_revol_lune_03_640.png){: .align-center}

Commencez par imprimer **le plateau** (en gris foncé) et insérez-le bien fermement au dessus du plateau précédents: les encoches doivent être clipsées à fond. Disposez ensuite les **4 axes** à imprimer en noir (à la verticale). Vous pouvez ensuite imprimer les **6 roues dentées**. J'ai imprimé la première de 13 dents en rouge car elle tourne à la vitesse de 1 jour par tour. Les autres roues peuvent s'imprimer en blanc.
{: .text-justify}
![Planétaire](/assets/images/tutos/038SoleilTerreLune/09_revol_lune_dessus_640.png){: .align-center}
> Assurez-vous bien que tous les mouvements soient bien fluides lorsque vous tournez la manivelle.
{: .text-justify}

### 10: Lune

![Planétaire](/assets/images/tutos/038SoleilTerreLune/10_lune_300.png){: .align-left}
La Lune est composée d'un plateau qui se fixe via un petit socle sur la roue de 81 dents du plateau précédent (il y a un petit renfort rectangulaire sur cette roue pour y fixer le plateau de la Lune) et de deux 1/2 sphères (lune_bas et lune_haut) à fixer sur l'axe vertical.
{: .text-justify}
{% include gallery id="gallery_lune" caption="Cliquez pour agrandir les images" %}


### 11: Terre
![Planétaire](/assets/images/tutos/038SoleilTerreLune/11_terre_300.png){: .align-left}
La Terre quand à elle se compose d'un **plateau**, de **deux roues de 28 dents** (attention elles ne sont pas identiques), **une collerette** , **deux 1/2 sphères** avec **une fixation** interne, et **un axe**. J'ai opté pour tout imprimer en bleu à l'exception des deux roues et la collerette que j'ai imprimées en rouge.
{: .text-justify}
{% include gallery id="gallery_terre" caption="Cliquez pour agrandir les images" %}
Le plateau se fixe au centre de la roue dentée. Attention à bien disposer la roue t**erre_gear28_axe_terre** sur l'axe de la terre, et l'autre sur l'axe verticale au milieu de la roue de 81 dents.
{: .text-justify}