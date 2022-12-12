---
#source: https://www.papsdroid.fr/post/terre
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
  teaser : "/assets/images/tutos/033Planetaire/terre.jpg"

#SEO tags
title       : "Planétaire: Terre"
image       : "/assets/images/tutos/033Planetaire/terre.jpg"
description : "Voici la mécanique qui permet à la Terre de prendre part à la danse hypnotisante des planètes autour du soleil"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Terre"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["planétaire" ]

gallery1:
  - image_path: "/assets/images/tutos/033Planetaire/terre-1.jpg"
  - image_path: "/assets/images/tutos/033Planetaire/terre-2.jpg"

---
![Planétaire](/assets/images/tutos/033Planetaire/terre.jpg){: .align-left}
Voici la mécanique qui permet à la Terre de prendre part à la danse hypnotisante des planètes autour du soleil. Cet article s'inscrit dans la longue série d'articles dédiés à mon [#planétaire](https://papsdroidfr.github.io/tags/#planétaire) mécanique. Je ne vous apprends rien en disant que la révolution de la Terre autour du Soleil dure **365,256 jours** (plus précisément 365,256363051 jours), cet article détaille la conception des engrenages imprimés 3D qui vont permettre d'atteindre ce résultat avec une grande précision.
{: .text-justify}

## Modèle 3D
![Planétaire](/assets/images/tutos/033Planetaire/terre3D-1.png){: .align-left}
Pour rappel, j'utilise un axe commun pour toutes les planètes qui représente 1 semaine (7 jours) par tour complet. Tous les mécanismes héritent de fait de ce premier rapport de réduction de 1/7. Il faut donc viser 7/365,256 comme rapport de réduction. J'utilise [mon programme](https://papsdroidfr.github.io/dev/CalculEngrenages/) développé en **python** pour simuler plusieurs solutions de manière à retenir le meilleur compromis entre précision et complexité (nombre d'étages d'engrenages). On arrive à une belle solution avec 3 étages avec 3 peties roues menantes de 13, 15 et 15 dents, contre 3 grandes roues menées de 37, 55 et 75 dents. Si vous faites le calcul 7 x ( 37x55x75) / ( 13x15x15) vous verrez que la précision est diaboliquement proche de 365,256.
{: .text-justify}

Si vous avez suivi les précédents prototypes concernant [Vénus](https://papsdroidfr.github.io/tutoriels/Planetaire-Venus/) et [Mercure](https://papsdroidfr.github.io/tutoriels/Planetaire-Mercure/), vous noterez que je n'ai utilisé que 2 étages. Mais pour la Terre avec seulement 2 étages on n'obtient pas une aussi belle précision qu'avec 3: Un étage de plus a pour conséquence **d'inverser le sens de rotation**, c'est la raison pour laquelle j'ai rajouté **une roue inverseuse** dès le départ (il y a une roue de 13 dents qui est entre la première roue menante de 13 et la première roue menées de 37). Ainsi on est sauvé, la Terre tourne dans le même sens que Mercure et Vénus.
{: .text-justify}
{% include figure image_path="/assets/images/tutos/033Planetaire/terre3D-2.png" caption="modèle 3D du système planétaire Soleil-Mercure-Vénus-Terre" %}

## Modèle réalisé et testé
Ça commence à être coton à assembler, mais quel régal de voir tous ces engrenages s'activer et danser la perfection des équations mathématiques. Il a fallu pas mal d'ajustements parfois au 10ème de mm près, et ces ajustements seront souvent nécessaires à chaque fois que je rajoute une couche supplémentaire: c'est la raison pour laquelle je ne partage pas encore les modèles 3D ( fichiers STL) tant que le projet n'est pas totalement finalisé.
{: .text-justify}
{% include gallery id="gallery1" caption="" %}
![Planétaire](/assets/images/tutos/033Planetaire/terre-3.jpg){: .align-center}

>La prochaine étape sera rouge !
