---
#source: https://www.papsdroid.fr/post/planetaire
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
  teaser : "/assets/images/tutos/033Planetaire/solar-system.jpg"

#SEO tags
title       : "Planétaire mécanique"
image       : "/assets/images/tutos/033Planetaire/solar-system.jpg"
description : "réalisation un planétaire mécanique du système solaire en impression 3D et commande électronique avec un Raspberry Pico."

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Planétaire"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pico" ,  "python3", "planétaire" ]

---

![Planétaire](/assets/images/tutos/033Planetaire/solar-system.jpg){: .align-left}
Je débute une série d'articles sous le tag #planétaire dans le but de réaliser un planétaire mécanique du système solaire en impression 3D et commande électronique avec un Raspberry Pico. Dans un but éducatif (démonstrations dans des écoles) je vais en réaliser deux: un système soleil-terre-lune permettant de d'expliquer et comprendre les phénomènes jour/nuit, les saisons, les éclipses, puis un autre avec les planètes du système solaire.
{: .text-justify}
Je vais détailler à travers plusieurs articles les étapes de conception, notamment toute la partie passionnante de mécanique à base d'engrenages qui relève de l'horlogerie, les modélisations et optimisations qui seront réalisées en Python, et la partie électronique bien entendue: tous les plans seront partagés sur le [Github du projet](https://github.com/papsdroidfr/planetaire) au fur et à mesure de l'avancement.
{: .text-justify}

## Principales fonctionnalités du planétaire
Le but est de construire une **maquette mécanique qui tient sur un bureau** (envergure de quelques dizaines de cm) pour visualiser les planètes tourner autour du soleil et pour le système soleil-terre-lune uniquement de visualiser la rotation de la terre sur elle même avec son inclinaison par rapport au plan de l'écliptique, ainsi que la lune en orbite autour de la terre.
{: .text-justify}
Concernant les autres planètes du système solaire je ne pense pas représenter leurs rotations sur elles-mêmes avec leurs inclinaisons (c'est déjà un tel casse-tête avec une seule planète....), ni leurs satellites à part notre Lune (on en compte plusieurs centaines de satellites: ça deviendrait un vrai bazar à gérer toutes ces rotations) On se contentera de voir le ballet des orbites autour du soleil avec tous les engrenages en action. 
{: .text-justify}
Le microcontrôleur pico me servira à commander l'éclairage du soleil, la gestion d'un moteur pas à pas avec plusieurs vitesses, et un affichage d'informations sur écran OLED
{: .text-justify}

## Prenons du recul avec les dimensions du système solaire
Ce premier article pose le décor des dimensions de notre système solaire, et nous allons vite comprendre qu'il faudra faire des simplifications, des choix, et que certaines proportionnalités ne pourront pas être respectées.
{: .text-justify}

### Orbites des planètes
![Planétaire](/assets/images/tutos/033Planetaire/ellipses.png){: .align-center}
Cette image peut être générée avec le script ellipse.py dans le dossier **/python** du [Github du projet](https://github.com/papsdroidfr/planetaire).
{: .text-justify}
L'orbite d'une planète a la forme d'une ellipse dont le soleil se trouve au centre de l'un des foyers. On mesure "l’aplatissement" d'une ellipse avec son paramètre d'excentricité "e" qui est égal :
{: .text-justify}

e = sqrt(1 - a²/b²)

où a est le grand axe de l'ellipse, et b le petit axe (sqrt étant la fonction racine carrée). Plus l'excentricité est proche de 0, et plus l'ellipse ressemble à un cercle. Plus elle est proche de 1, plus l'ellipse sera "aplatie". 
{: .text-justify}
La première ligne dans l'image ci-dessus représente des ellipses témoin avec plusieurs excentricités (la première ellipse étant un cercle e=0). Celle du dessous représente les ellipses des planètes du [système solaire](https://fr.m.wikipedia.org/wiki/Syst%C3%A8me_solaire) (source wikipedia). Mercure est la planète qui a la plus grosse excentricité: e(mercure)=0,206: c'est si peu aplati qu'on dirait un cercle!, ainsi que toutes les autres planètes dont l'excentricité est ridiculement faible.
{: .text-justify}
>**Première simplification**: à l'échelle de notre bureau, les orbites seront des cercles et personne n'y verra de différence. Ça nous arrange bien car représenter un système mécanique avec des ellipses bonjour le casse tête... 
{: .text-justify}

### Taille des orbites 
Concernant la taille des orbites et des planètes nous voilà face à un vrai dilemme si on veut respecter la contrainte de taille de l'ordre de quelques dizaines de centimètres.
{: .text-justify}
![Planétaire](/assets/images/tutos/033Planetaire/orbites.png){: .align-left}
Cette image est générée avec le script **taille_planetes.py** (dossier **/python** du [Github du projet](https://github.com/papsdroidfr/planetaire)). En haut à gauche sont représentées à l'échelle toutes les orbites des 8 planètes (dans l'ordre en distance au soleil: Mercure, Venus, Terre, Mars, Jupiter, Saturne, Uranus, Neptune): elles s'étalent à quelques 5 milliard de km du soleil. On voit tout de suite le problème: les orbites des 4 planètes les plus éloignées sont visibles, tandis que les orbites de Mercure, Vénus, la Terre et Mars sont contenues dans une tête d'épingle! L'image en haut à droite zoome sur les orbites à moins de 230 millions de km du soleil (Mercure, Vénus, la Terre et Mars): on va pouvoir conserver les proportions pour celles-ci, mais pour les 4 autres plus éloignées il va falloir "tricher" avec les proportions. A titre d'exemple la terre est à 149,6 millions de km du soleil, et Neptune la plus éloignée est à 4,5 milliard de km. Si je représente la terre à 10 cm du soleil dans mon planétaire, en voulant conserver les proportions il me faudrait placer Neptune à .... 3m !
{: .text-justify}
>**Deuxième simplification**: on ne conservera pas les proportions des orbites de Jupiter, Saturne, Uranus et Neptune. On va se contenter de les placer "plus loin" dans le bon ordre.
{: .text-justify}

### Taille des planètes
Enfin concernant la taille des planètes (les diamètres évoluent entre 4878 km pour Mercure et 142984 km pour Jupiter en passant par 12756km pour la Terre), dont on peut voir l'image dans le graphe ci-dessous (sous les orbites): même combat, et encore je n'ai même pas représenté le soleil qui est grosso modo 10 fois plus gros que Jupiter déjà elle même 10 fois plus grosse que la Terre! Si on veut conserver les proportions, on va soit se retrouver avec des planètes hors-norme en taille pour notre maquette (qui doit tenir sur un bureau), ou alors avec des tètes d'épingle invisibles pour les planètes les plus proches du soleil dont la Terre.
{: .text-justify}
>**Troisième simplification**: on ne conservera pas les proportions des tailles des planètes de Jupiter, Saturne, Uranus et Neptune (décidément elles nous embêtent ces 4 là), on va se contenter de les représenter plus ou moins grosses en respectant l'ordre des tailles respectives.
{: .text-justify}

### Révolution des planètes
![Planétaire](/assets/images/tutos/033Planetaire/revolutions.png){: .align-left}
Ce qu'il faut respecter impérativement par contre ce sont les périodes de révolution des planètes autour du soleil qui varient de 88 jours pour Mercure à 165 ans pour Neptune. Encore une fois les 4 Jupiter, Saturne, Uranus et Neptune écrasent toute proportion ! Si par exemple sur notre maquette Mercure tourne en 20 secondes autour du soleil (ce qui est très rapide pour une maquette mécanique: n'oublions pas que la terre va tourner sur elle-même 88 fois plus vite que Mercure autour du Soleil!), et bien Neptune mettra environ 4h, Uranus 2h, Saturne 42mn et Jupiter 17mn. 
{: .text-justify}
> Je pense qu'Uranus et Neptune vont être écartées de notre planétaire car on les verrait à peine bouger. Ce sont d'ailleurs les deux planètes qu'on ne peut pas voir à l’œil nu tellement elles sont loin.
{: .text-justify}

Voilà donc pour conclure, notre planétaire mécanique à l'échelle d'un bureau va:
{: .text-justify}
* représenter les orbites sous forme de cercles
* tricher sur les proportions des orbites et taille de Jupiter et Saturne (et du soleil)
* intégrer les 6 planètes visibles à l’œil nu: Mercure, Vénus, la Terre (et son satellite la lune), Mars, Jupiter et Saturne.

> Le prochain article traitera des systèmes mécaniques à engrenages avec les calculs de démultiplication et de précision.
{: .text-justify}
