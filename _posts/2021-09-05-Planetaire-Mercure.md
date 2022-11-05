---
#source: https://www.papsdroid.fr/post/mercure
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
  teaser : "/assets/images/tutos/033Planetaire/Mercure.jpg"

#SEO tags
title       : "Planétaire: Mercure"
image       : "/assets/images/tutos/033Planetaire/Mercure.jpg"
description : "Ce tout premier prototype avec Mercure est imprimé en PLA. Il me permet de valider le choix des dimensions et fixations des engrenages pour mon planétaire mécanique"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Mercure"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["planétaire" ]

gallery1:
  - image_path: "/assets/images/tutos/033Planetaire/mercure3D01.png"
  - image_path: "/assets/images/tutos/033Planetaire/mercure3D02.png"

---
![Planétaire](/assets/images/tutos/033Planetaire/Mercure.jpg){: .align-left}
Ce tout premier prototype avec Mercure est imprimé en PLA. Il me permet de valider le choix des dimensions et fixations des engrenages pour mon planétaire mécanique. J'ai du faire de nombreux essais avant d'arriver à un résultat satisfaisant qui soit insensible aux variations et vibrations quand toute la mécanique s'active.
{: .text-justify}

## Conception 3D
![Planétaire](/assets/images/tutos/033Planetaire/mercure3D03.png){: .align-center}
{% include gallery id="gallery1" caption="Tout a été conçu avec le logiciel [FreeCAD](https://www.freecadweb.org/?lang=fr) et **l'add-on Gear** pour modéliser facilement les roues dentées." %}

### les roues
![Planétaire](/assets/images/tutos/033Planetaire/mercure-roues.png){: .align-left}
Les roues dentées modélisées avec l'add-on GEAR de FreeCAD sont épaisses de 3mm. J'ai pris l'option d'engrenages classiques avec un profil de dents en développante de cercles. Le centre a un profil hexagonal pour pouvoir les loger sur un axe dont la rotation est solidaire à la roue. L'intérieur de la roue est évidé pour ne laisser que 4 bras centraux afin d'éviter d'imprimer trop de matière. Enfin pour m'y retrouver avec le nombre de dents, j'ai disposé des marquages: les petits rectangles représentent 10 dents (ils ne sont présents qu'à partir de 20 dents), les petits cercles 1 dent. Ainsi d'un coup d’œil je sais combien la roue a de dents quand je me retrouve à devoir assembler toutes les pièces du puzzle (sur les photos: une roue de 15 dents et une autre de 91 dents). Enfin, au dessus du centre hexagonal j'ai disposé une petite collerette de 1mm d'épaisseur et 2mm de large afin de limiter les frottements horizontaux de la roue uniquement sur cette petite zone. La base de la roue est parfaitement plate, afin de pouvoir l'imprimer sans aucun support.
{: .text-justify}

### Les axes
![Planétaire](/assets/images/tutos/033Planetaire/mercure-axes.png){: .align-left}
Les axes ont tous un profil hexagonal d'une taille diminuée de 0.2mm par rapport aux centres hexagonaux des roues et avec un chanfrein de 1mm sur les arrêtes verticales pour pouvoir les rentrer facilement sans que ce soit trop lâche non plus. Certains axes sont pleins, et d'autres ont un creux de profil circulaire à l'intérieur afin de pouvoir y loger un autre axe hexagonal plus petit qui aura sa propre vitesse de rotation indépendante. En effet les planètes tournent toutes autour du même centre (le soleil) et elles ont toutes leur propre vitesse de rotation: il me faut donc concevoir des axes qui s'emboîtent comme des poupées russes les uns dans les autres. Ces axes avec un profil creux grossissent de 2mm de rayon à chaque fois (les parois de 2mm d'épaisseur sont bien assez solides, en dessous c'est limite). Le premier axe qui supporte le Soleil est plein, 4mm de rayon. Celui qui supporte la rotation de Mercure fait donc 6mm de rayon avec l'axe du Soleil qui reste fixe à l'intérieur. Ensuite viendra l'axe de Vénus 8mm de rayon autour de celui de Mercure, puis la Terre 10mm de rayon autour de Vénus etc ... jusqu'à l'axe de Saturne (6ème planète) qui fera donc 16mm de rayon avec tous les autres à l'intérieur en poupée russe. 
{: .text-justify}

### Les logements et supports
![Planétaire](/assets/images/tutos/033Planetaire/mercure-supports.png){: .align-left}
Ces logements d'une épaisseur de 3mm vont maintenir tous les axes et roues en position. Ils sont fixés entre eux avec des petits axes hexagonaux qui entrent "de force" dans des trous hexagonaux. Les axes des roues quand à eux sont logés dans des trous circulaires, les axes étant d'un rayon 0.2mm plus petit avec arrêtes chanfreinées: la rotation est parfaitement fluide. Certains trous sont traversants, d'autres non (profond de 2mm sur les supports épais de 3mm). Si une roue doit être positionnée, une petite collerette de 1mm de hauteur (ou 5mm selon la position de la roue) et 2mm de large est rajoutée autour de chaque trou circulaire afin de limiter les zones de frottement horizontaux des roues, et aussi pour rajouter de la profondeur pour les logements des axes qui doivent rester le plus vertical possible pendant leur rotation.
{: .text-justify}

### Les étages d'engrenages
![Planétaire](/assets/images/tutos/033Planetaire/mercure-etages.png){: .align-left}
Mercure tourne autour du soleil en **87.969 jours** (arrondi à la troisième décimale). Je pourrais me contenter d'arrondir à 2 décimales personne n'y verrai beaucoup de différence.... Pour calculer les engrenages qui arrivent à ce résultat, j'ai utilisé [ce programme de simulation](https://www.papsdroid.fr/post/planetaire-calcul-engrenages) en python qui donne les meilleurs résultats avec 2, 3 ou 4 étages. Par contre j'ai décidé d'utiliser un 1er étage de 13 et 91 roues commun à tous les mécanismes. 13 * 7 = 91 donc un si un tour de manivelle représente une journée, et bien un tour de la 1ère grande roue représente pile une semaine. **Tous les mécanismes vont hériter de ce 1er rapport de réduction 1/7**, si bien qu'il me suffit de calculer les rapports de réduction divisés par 7 à chaque fois. Je ne retiens pas forcément le meilleur résultat qui va demander parfois 4 étages de plus pour arriver à égaler jusqu'à la 6ème décimale: approcher la 3ème décimal au plus près suffit. Pour Mercure j'ai retenu comme **roues menantes [13, 14, 15]** et comme **roues menées [91, 29, 91]**. Si vous faites le calcul 91x29x91 / (13x14x15) vous tomberez sur 87.967 arrondi à la 3ème décimale: soit une erreur de 0.0022% ! En faisant tourner ce mécanisme en temps réel (24h pour tourner une seule fois la manivelle qui représente 1 jour), et bien il faudrait patienter environ 90 jours pour commencer à voir un écart d'une minute apparaître !
{: .text-justify}

### Le plateaux tournant et planètes
![Planétaire](/assets/images/tutos/033Planetaire/mercure-plateaux.png){: .align-left}
Enfin dernières pièces du puzzle: le plateau tournant pour les planètes. Les planètes et le soleil sont représentées par une simple sphère aplatie en bas pour faciliter l'impression 3D, avec un trou hexagonal en rapport avec son axe hexagonal. La surface haute du trou hexagonal a été chanfreiné pour pouvoir imprimer ces sphères **sans aucun support**, sinon on se retrouverait avec une surface plate à imprimer dans le vide qui impose de prévoir des supports. Les plateaux sont solidement axés sur leurs axes hexagonaux, posés sur la petite collerette de 1mm de haut des supports plats afin de limiter les zones de frottements. Rappelez-vous que les axes des planètes sont disposés les un sur les autres comme des poupées russes, chacun tournant à sa propre vitesse (fixe pour le soleil). Il suffit de les faire dépasser les un après les autres au dessus de tous les mécanisme à engrenages pour pouvoir y poser le plateau sans que sa rotation gêne. Il faut bien entendu commencer par faire dépasser l'axe du soleil au plus haut, ensuite les planètes de plus en plus éloignées: Mercure en premier etc.. Saturne en dernier donc, et le tour est joué: aucun plateau ne gênera ni les mécanismes de réduction, ni les autres plateaux, et les frottements seront à chaque fois limités sur une collerette de 1mm d'épaisseur et 2mm de large.
{: .text-justify}

## Vidéo du prototype
J'ai choisi pour ce projet de ne travailler exclusivement qu'avec des **fabriquants de PLA Français** qui respectent bien les normes de protection de l'environnement et de sécurité pour notre santé (pas de bisphénol, pas d'additifs dangereux, circuit de production court), j'ai pris du PLA effet métallique Bronze (pour les roue), Argent (pour les axes) et Or (support) .
{: .text-justify}
{% include video id="XA_sy-n--_k" provider="youtube" %}

>Je publierai les fichiers STL 3D quand tout le projet sera fini, car il y aura sûrement quelques adaptions à faire pour que l'équilibre global soit bien au rendez-vous quand je vais rajouter les mécanismes des 5 autres planètes que sont Vénus, la Terre, Mars, Jupiter et Saturne.
{: .text-justify}