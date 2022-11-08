---
#source: https://www.papsdroid.fr/post/_mars
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
  teaser : "/assets/images/tutos/033Planetaire/mars.jpg"

#SEO tags
title       : "Planétaire: Mars"
image       : "/assets/images/tutos/033Planetaire/mars.jpg"
description : "Place à la planète Mars de s'intégrer dans mon planétaire mécanique. Elle tourne autour du soleil en 686,980 jours."

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Mars"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["planétaire" ]

gallery1:
  - image_path: "/assets/images/tutos/033Planetaire/mars3D-2.png"
  - image_path: "/assets/images/tutos/033Planetaire/mars3D-3.png"

gallery2:
  - image_path: "/assets/images/tutos/033Planetaire/mars3D-4.png"
  - image_path: "/assets/images/tutos/033Planetaire/mars3D-5.png"

gallery3:
  - image_path: "/assets/images/tutos/033Planetaire/mars1.jpg"
  - image_path: "/assets/images/tutos/033Planetaire/mars2.jpg"
  - image_path: "/assets/images/tutos/033Planetaire/mars3.jpg"

---
![Planétaire](/assets/images/tutos/033Planetaire/mars.jpg){: .align-left}
Place à la planète Mars de s'intégrer dans mon [#planétaire](https://papsdroidfr.github.io/tags/#planétaire) mécanique. Elle tourne autour du soleil en **686,980 jours**, et nous obtiendrons avec cette mécanique une révolution en **686,981 jours**, avec 2 étages d'engrenages dédiés, qui partent comme pour toutes les autres planètes de l'axe de rotation hebdomadaire commun, pour démultiplier l'axe central de rotation de Mars centré sur l'axe du soleil, en parfaite synchronisation avec toutes les autres planètes.
{: .text-justify}

## Modélisation 3D
![Planétaire](/assets/images/tutos/033Planetaire/mars3D-1.png){: .align-left}
Pour parvenir à ce résultat, j'ai dans un premier temps utilisé mon [simulateur Python](https://www.papsdroid.fr/post/planetaire-calcul-engrenages) de calcul des rapports de réduction en retenant le meilleur compromis entre précision et complexité: je trouve un très bon compromis avec seulement 2 étages de 3 roues menantes de 14, 15 et 16 dents contre 3 roues menées de 59, 69 et 81 dents. Je rappelle que la première roue menante (celle de 14 dents en l’occurrence) est positionnée sur l'axe de rotation des semaines: il faut donc multiplier le tout par 7 pour avoir le rapport de réduction final: 7x(59x69x81)/(14x15x16) = **686,98125** : pas la peine de se compliquer la vie en rajoutant un 3me étage pour gagner en précision: c'est déjà très précis comme ça quand on vise à obtenir 686.980 ! Enfin une dernière petite astuce pour que Mars tourne dans le même sens que les autres planètes: je pars de l'axe de rotation hebdomadaire **déjà inversé** du mécanisme de la Terre. L'axe central qui soutient le bras sur lequel Mars est positionnée est quand à lui centré sur l'axe du soleil, et enveloppe les axes respectifs de Mercure, Vénus et celui de la Terre: ils sont tous à l'intérieur.
{: .text-justify}
{% include gallery id="gallery1" caption="" %}
{% include gallery id="gallery2" caption="Modèle 3D du système complet avec le Soleil, Mercure, Vénus, la Terre, et Mars. " %}

## Prototype réalisé
Je ne publie pas encore les STL car le moindre petit ajustement sur une pièce (plus particulièrement les supports, et le positionnement des axes) se propage en cascade sur de nombreuses autres pièces à refaire. Ce prototype est réalisée de manière itérative: chaque nouvel élément pouvant impacter les autres, même si j'essaye dans la mesure du possible d'éviter à devoir tout refaire à chaque fois. Par exemple en ajustant les engrenages de Mars, je me suis rendu compte que j'avais mal positionné à quelque 1/10 de mm près un axe d'un engrenage du mécanisme de la Terre, ce qui m'engendrait des problèmes de blocage de certaines roues "trop serrées". Ça ne se ressentait pas trop avec Mercure, Vénus et la Terre, mais en rajoutant les étages d'engrenages de Mars j'ai bien cru que j'allais devenir fou avec les réglages. Cet axe traversant quasiment tous les supports à la verticale: il m'a fallu tous les refaire !
{: .text-justify}
{% include gallery id="gallery3" caption="" %}
>Ce prototype imprimé en résolution 0.2mm fonctionne très bien, les rotations des engrenages sont parfaitement fluides. Je vais désormais attaquer la motorisation complète du circuit.
{: .text-justify}
