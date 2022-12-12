---
#source: https://www.papsdroid.fr/post/planetaire-maquette
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
  teaser : "/assets/images/tutos/033Planetaire/maquette/planetaire-300.png"

#SEO tags
title       : "Planétaire: maquette imprimée 3D"
image       : "/assets/images/tutos/033Planetaire/maquette/planetaire-300.png"
description : "Cet article détaille l'assemblage de la maquette du planétaire mécanique composée des 4 planètes intérieures (Mercure, Vénus, la Terre et Mars) qui tournent autour du Soleil, avec en option la motorisation du système"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Maquette"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["planétaire" ]

gallery1:
  - image_path: "/assets/images/tutos/033Planetaire/maquette/03Mercure.png"
  - image_path: "/assets/images/tutos/033Planetaire/maquette/04Mercure.png"

gallery2:
  - image_path: "/assets/images/tutos/033Planetaire/maquette/05Venus.png"
  - image_path: "/assets/images/tutos/033Planetaire/maquette/06Venus.png"

gallery3:
  - image_path: "/assets/images/tutos/033Planetaire/maquette/07Terre.png"
  - image_path: "/assets/images/tutos/033Planetaire/maquette/08Terre.png"

gallery4:
  - image_path: "/assets/images/tutos/033Planetaire/maquette/15Soleil.png"
  - image_path: "/assets/images/tutos/033Planetaire/maquette/16Soleil.png"

gallery5:
  - image_path: "/assets/images/tutos/033Planetaire/maquette/17Motor.png"
  - image_path: "/assets/images/tutos/033Planetaire/maquette/18Motor.png"

---
![Planétaire](/assets/images/tutos/033Planetaire/maquette/planetaire-300.png){: .align-left}
Cet article détaille l'assemblage de la maquette du [#planétaire](https://papsdroidfr.github.io/tags/#planétaire) mécanique. mécanique composée des 4 planètes intérieures **(Mercure, Vénus, la Terre et Mars)** qui tournent autour du **Soleil**, avec en option la motorisation du système. Il faudra s'armer de patience car il y a 63 pièces à imprimer en 3D pour assembler ce gros engrenage: des supports, des axes, des roues dentées de 13 à 99 dents, les planètes, le Soleil et quelques collerettes de maintien: **100% imprimé 3D** sans aucune vis (à par celles pour fixer le moteur au support), le tout étant animé en respectant les périodes de rotation. La maquette est parfaitement fonctionnelle sans le moteur.
{: .text-justify}

## Conseils d'impression
Tous les fichiers 3D sont fournis dans le [Github du projet](https://github.com/papsdroidfr/planetaire), dossier **/STLPlanetaire**. Il y a des sous-dossiers correspondants aux étapes ci-dessous. **Je déconseille de tout imprimer d'un coup!** vous allez vous retrouver avec un gros tas d'axes et de roues dentées qui vont être galère à retrouver et assembler dans le bon ordre: mieux vaut imprimer les pièces d'une seule étape (en commençant par l'étape 01), de les assembler, et ainsi de suite jusqu'à la dernière étape.
{: .text-justify}
>**Petite astuce:** les roues dentées sont marquées (à l'exception de la petite roue de 13 dents) avec des rectangles sur l'intérieur (sauf les petites roues de 13 à 16 dents) et des cercles sur les bords extérieurs: les rectangles matérialisent 10 dents, les cercles 1 dent. Ainsi en comptant les rectangles et les cercles, on voit tout de suite combien il y a de dents sans avoir à les compter une par une ...
{: .text-justify}

Pour ce qui est des impressions, tout s'imprime parfaitement en **0,2mm** et pas besoin d'activer les supports à l'exception d'une seule pièce que je préciserai. J'ai trouvé qu'il était préférable d'imprimer les axes **à la verticale** plutôt qu'à l'horizontale: ils sont ainsi moins déformés sur leur base. Tout est calculé à 0,2mm près, donc il ne faut pas hésiter à utiliser un cutter pour se débarrasser des petites imperfections qui viennent gêner les rotations des axes.
{: .text-justify}

## Étapes de construction
### Étape 01: le pied
![Planétaire](/assets/images/tutos/033Planetaire/maquette/01Pied.png){: .align-left}
Imprimer le support bas, les 4 axes de fixation hexagonaux (ils sont "fixes" et vont maintenir les supports figés entre eux) ainsi que l'axe central mobile: cet axe va simuler 1 semaine entière (7 jours) lors d'une rotation complète. La motorisation va permettre de décomposer une rotation complète en 7 étapes pour compter les jours qui passent.
{: .text-justify}

### Étape 02: le Soleil
![Planétaire](/assets/images/tutos/033Planetaire/maquette/02Soleil.png){: .align-left}
Imprimer ensuite le Soleil et son axe de fixation, ainsi que sa collerette de maintien. Vous pouvez mettre en place l'axe de fixation, mais **mettez de côté pour l'instant le Soleil et sa collerette de maintien** car ils vont vous empêcher d'assembler la suite: il faudra les assembler en tout dernier.
{: .text-justify}

### Étape 03: Mercure
{% include gallery id="gallery1" caption="" %}
La première image montre les éléments à imprimer et assembler concernant l'étage de Mercure. Il faut imprimer Mercure, son plateau de maintien, son axe central et sa collerette de maintien, 4 roues dentées de 14, 15, 29 et 91 dents, une petite collerette à disposer sur la roue de 14 dents, un support bas et un petit axe de rotation. Assemblez tous les éléments **sauf Mercure, son plateau et sa collerette de maintien** sur le pied précédemment assemblé: la deuxième image montre le résultat.
{: .text-justify}
L'ordre des roues dentées est le suivant: à gauche (sur l'axe principal des semaines):14 dents avec une petite collerette au dessus, ensuite à côté la roue de 29 dents avec son axe, puis au dessus une roue de 15 dents, et à droite la grosse roue de 91 dents fixée sur l'axe de rotation de Mercure. Cet axe vient englober l'axe central du Soleil.
{: .text-justify}
Pour comprendre la sorcellerie qui se cache derrière ce choix de roues dentées qui permet d'obtenir une période de rotation très précise, lisez [cet article](https://papsdroidfr.github.io/tutoriels/Planetaire-Mercure/).
{: .text-justify}
>Avant d'assembler la suite, assurez vous en faisant tourner à la main l'axe des semaines que tout tourne de manière fluide sans accroche.
{: .text-justify}

### Étape 04: Vénus
{% include gallery id="gallery2" caption="" %}
La première image montre tous les éléments à imprimer concernant l'étage de Vénus. Il y a le support bas, 4 roues dentées : 2x16 dents, 1x83 dents et 1x99 dents (se sera la plus grosse roue dentée de tout le mécanisme), une petite collerette, un axe de fixation, le plateau de support de planète, l'axe central de Vénus, et bien entendu Vénus. Comme précédemment il faut tout assembler **sauf Vénus et son support** qui seront à mettre plus tard (la seconde image montre le résultat).
{: .text-justify}
L'ordre des roues dentées dont le choix est expliqués [ici](https://papsdroidfr.github.io/tutoriels/Planetaire-Venus/) est le suivant: une petite roue de 16 dents qui s'insère directement sur l'axe des semaines, puis une grosse roue adjacente et déportée de 83 dents sur laquelle est fixée une autre petite roue de 16 dents, adjacente pour finir à la grosse roue de 99 dents qui entraîne l'axe de rotation central de Vénus étant disposé autour de l'axe de rotation de Mercure: tous les axes des planètes s'emboîtent les un sur les autres autour de l'axe central du Soleil.
{: .text-justify}
>Avant de passer à l'étape suivante assurez-vous que **tout tourne de manière fluide**. Pour la suite, vous pourrez utiliser cette roue déportée de 83 dents pour entraîner tout le mécanisme: c'est plus pratique que de chopper le petit axe des semaines.
{: .text-justify}

### Étape 05: La Terre
{% include gallery id="gallery3" caption="" %}
L'étage de notre chère Terre nécessite un engrenage plus complexe à imprimer. Il est composé de 7 roues dentées de 2x13 dents, 2x15 dents, 1x37 dents, 1x55 dents et 1x75 dents! Pour les maintenir il faudra imprimer 4 axes, 2 collerettes identiques,  le support bas qui nécessite d'activer les supports à l'impression (à cause du trou hexagonal situé dessous en bas à gauche), l'axe central de la Terre, le plateau ainsi que la Terre.
{: .text-justify}
Comme précédemment tout s'assemble **sauf le plateau et la Terre** à réserver en dernier. Les roues dentées à assembler sont expliquées dans [cet article](https://papsdroidfr.github.io/tutoriels/planetaire-terre/). Vous remarquerez que l'axe des semaines est adjacent à un autre axe juste à côté dont l'effet et de tourner dans le sens inverse afin d'avoir toutes les planètes qui tournent dans le même sens. L'ordre des roues est le suivant: deux petites roues de 13 dents adjacentes (sur lesquelles on dispose les 2 collerettes), puis une roue de 37 dents sur laquelle il faut mettre une roue de 16 dents sur un tout petit axe. Déportée vers le bas se trouve ensuite la roue de 55 dents sur laquelle il y a la seconde roue de 16 dents avec un tout petit axe. Enfin la dernière roue de 75 dents entraîne l'axe de rotation central de la Terre qui s'emboîte sur l'axe de rotation de ... Vénus.
{: .text-justify}
**Vérifiez bien la fluidité du mécanisme** avant d'assembler la suite, en utilisant la roue déportée de 83 dents de Vénus. Si les roues et axes ne sont pas stables c'est normal car ils ne sont pas encore maintenus par le support de l'étape suivante. 
{: .text-justify}
>Si vous essayez de faire tourner le mécanisme en partant des grosses roues qui entraînent les axes centraux de Vénus ou de la Terre directement, **vous aurez du mal et c'est normal** car l'accélération des roues en cascade vers l'axe des semaines impose un **gros effort de couple**: à ne pas faire vous allez péter le mécanisme, il n'est pas conçu pour résister à un tel couple.
{: .text-justify}

### Étape 06: Mars
![Planétaire](/assets/images/tutos/033Planetaire/maquette/09Mars.png){: .align-center}
![Planétaire](/assets/images/tutos/033Planetaire/maquette/10Mars.png){: .align-center}
![Planétaire](/assets/images/tutos/033Planetaire/maquette/11Mars.png){: .align-center}
Concernant l'étage de Mars il faudra imprimer 2 supports (1 bas et 1 haut qui vient refermer tout le mécanisme), 6 roues dentées (14 dents, 15 dents, 16 dents, 59 dents, 69 dents et 81 dents), 2 axes de fixation qui viennent maintenir les 2 supports haut et bas, 2 axes de rotation pour les roues, une collerette qui sert à surélever le plateau de Mars, l'axe central de Mars, son plateau, et pour finir Mars. **Attention** pour imprimer correctement le support haut il faut le **retourner à 180°** à cause de la colerette intégrée en bas (le dessus est plat).
{: .text-justify}
Le choix des roues dentées est expliqué dans [cet article](https://papsdroidfr.github.io/tutoriels/planetaire-mars/). La première petite roue de 14 dents est positionnée sur l'axe des semaine "inversé". Il entraîne alors une roue déportée en bas à gauche de 59 dents sur laquelle il faut mettre une petite roue de 15 dents, qui à son tour entraîne une grosse roue de 69 dents par dessous laquelle il faut mettre la petite roue de 16 dents, elle entraîne à son tour une grosse roue de 81 dents fixée sur l'axe central de Mars. L'image n°2 montre la disposition du support haut qui vient fermer le mécanisme (pensez à bien rajouter les deux axes de fixation dans les logements hexagonaux prévus). Pour positionner le plateau de Mars il faut d'abord y mettre dessous la collerette qui va le sur-élever un peu pour éviter que le bras de maintien de Mars ne vienne percuter le mécanisme. Cette fois vous pouvez assembler le plateau et Mars au mécanisme, et vérifier que tout tourne de manière fluide en actionnant la roue déportée de l'étage de Vénus.
{: .text-justify}

### Étape 07: assemblage des planètes
{% include gallery id="gallery4" caption="" %}
C'est presque fini: il reste à assembler dans l'ordre les plateaux et les planètes: après avoir mis la collerette autour de l'axe de Mars, son plateau et Mars vous pouvez assembler le plateau de la Terre avec la Terre, puis le plateau de Vénus avec Vénus, enfin de plateau de Mercure avec Mercure et sa collerette de maintien (elle sert à éviter que les plateaux ne cambrent trop lors des rotations), pour finir par la collerette de maintien du Soleil, le Soleil étant la dernière pièce à poser.
{: .text-justify}
>Le mécanisme doit tourner de manière fluide en actionnant la roue déportée de Vénus (celle qui est marquée d'une flèche sur la seconde image).
{: .text-justify}
Pour me faciliter la vie, vous aurez remarqué que j'ai préféré prendre des screenshot d'images 3D plutôt que de vraies photos qui m'auraient obligées à démonter et remonter toute la mécanique, et exigé beaucoup de temps pour trouver les bons angles de prise de vue.
{: .text-justify}

### Étape 08 optionnelle: motorisation
{% include gallery id="gallery5" caption="" %}
Concernant la motorisation , cela sous-entend qu'il faudra aussi fabriquer **le boîtier de commande** à base d'un Raspberry PICO, expliqué dans [cet article](https://papsdroidfr.github.io/tutoriels/planetaire-motor/). Encore une fois la maquette fonctionne sans le moteur. La motorisation sert uniquement à ajouter un peu de fun au système pour simuler une journée, une semaine, un mois ou un fonctionnement en continue avec l'affichage sur un petit écran LCD du nombre de jours qui passent.
{: .text-justify}
![Planétaire](/assets/images/tutos/033Planetaire/maquette/19Motor.png){: .align-center}
Imprimez d'abord le support du moteur (qui sera à viser sur le moteur avec 4 petites vis M3 du type celles qu'on utilise dans les ordinateurs pour viser les disques durs etc ...), puis 2 petites roues dentées dont les dents sont inclinées à 45° (la plus fine va sur l'axe du moteur), un petit axe. Le moteur ainsi fixé sur son support avec les roues dentées se loge naturellement en bas du mécanisme comme le montre la dernière photo.
{: .text-justify}
![Planétaire](/assets/images/tutos/033Planetaire/motor-assemblage3.jpg){: .align-center}
>Photo du système complet motorisé qui fonctionne chez moi. Je l'ai testé durant plusieurs heures en continue, en totalisant plus de 35 000 jours de rotation.
{: .text-justify}
