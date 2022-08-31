---
#source: https://www.papsdroid.fr/post/monitoring-syst%C3%A8me-unicorn-hat-hd
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
  teaser : "/assets/images/tutos/003sysdroid/02sysdroidUHHD/sysdroid_uhhd_300.jpg"

#SEO tags
title       : "Sysdroid Unicorn HAT HD"
image       : "/assets/images/tutos/003sysdroid/02sysdroidUHHD/sysdroid_uhhd_300.jpg"
description : "Monitoring Raspberry pi avec une matrice 256 leds de Pimoroni"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Sysdroid"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pi" ,  "python3",  "HAT", "Pimoroni", "matrice leds", "monitoring"]

---

![sysdroid-UHHD](/assets/images/tutos/003sysdroid/02sysdroidUHHD/sysdroid_uhhd_300.jpg){: .align-left} 
Monitoring Raspberry pi avec une matrice 256 leds de Pimoroni.
<!--more-->
Dans ce tutoriel on va oublier le fer à souder et utiliser cette fabuleuse matrice de 256 leds proposée au format HAT par [Pimoroni](https://shop.pimoroni.com/): la [Unicorn HAT HD](https://shop.pimoroni.com/products/unicorn-hat-hd). Il s'agit d'une autre version de mon [sysdroid](https://papsdroidfr.github.io/tutoriels/sysdroid/) qui consiste à monitorer l'occupation des CPUS, de la RAM, de l'espace disque, ainsi que la température du CPU en temps réel, en occupant le moins de ressource possible pour ne pas perturber le Raspberry pi.
{: .text-justify}

## La Unicorn HAT HD
![sysdroid-UHHD](/assets/images/tutos/003sysdroid/02sysdroidUHHD/20190916_212618_web.jpg){: .align-center} 
Commençons par présenter la bête: c'est une carte au format HAT doté d'une matrice de 16*16 leds RGB. Pimoroni fourni une bibliothèque Python qui permet de la gérer très simplement. Des programmes démonstratifs sont fournis: voilà une démo particulièrement spectaculaire qui annonce les possibilités de cette matrice !
{: .text-justify}

{% include video id="lmwvMEqteGI" provider="youtube" %}

Dans ce tutoriel nous n'aurons besoin que de cette matrice, livrée avec un diffuseur du plus bel effet à visser sur la matrice, d'installer la bibliothèque en suivant le [guide d'installation](https://github.com/pimoroni/unicorn-hat-hd), puis de récupérer sur ma [page Github](https://github.com/papsdroidfr/sysdroidUHHD) les programmes python3 qui vont monitorer le système et afficher le résultat sur la matrice.
{: .text-justify}

## programmes python3

En premier lieu je conseille d'exécuter le programme **uhhd_test.py**: il va simultanément allumer toutes les leds en rouge, puis vert, puis bleu avec 2 secondes d'attente, et recommencer jusqu'à appui sur CTRL-C. Ça a l'air idiot ce test mais pas tant que ça... car il permet d'identifier une éventuelle led morte qui se voit tout de suite avec ce programme. Dans ce cas: faites une photo, et contactez le support après vente ils sont très réactif, c'est du vécu ! 
{: .text-justify}

Deux programmes python3  permettent l'exécution du sysdroid sur la matrice Unicorn HAT HD:

**sysdroid_main.py**: c'est le programme principal à exécuter. Il est composé de deux classes
- **classe ReadSys** qui hérite du mutli-threading: elle va lire toutes les secondes les informations systèmes telles que occupation CPU, RAM, Disque, et température CPU et stocker le résultat. Comme c'est un thread, elle est exécutée en tâche de fond.
{: .text-justify}
- **classe Sysdroid**, qui hérite aussi du mutli-threading. Elle va lire les informations préparées par une instance de ReadSys et afficher les informations sur la matrice sous forme de barres de niveau verticales, et texte.
{: .text-justify}
- **classe Application**: c'est l'application principale qui va instancier un Sysdroid et le faire démarrer en tâche de fond, et ne rien faire d'autre pour ne pas saturer les processeurs.
{: .text-justify}

Vous pouvez utiliser plusieurs paramètres utiles: le **délais de rafraîchissement** (par défaut toutes les secondes) et un **indicateur de rotation** pour orienter l'affichage à 0°, 90°, 180° ou 270° : tout autre angle de rotation sera rapproché à un multiple de 90° le plus près. Cet angle de rotation permet d'orienter l'affichage en fonction de l'orientation du Raspberry pi: super pratique !
{: .text-justify}

**sysdroid_uhhd.py**: il est composé de deux classes qui vont gérer les affichages souhaités sur la matrice en exploitant la bibliothèque Python fournie par Pimoroni.
{: .text-justify}
- **classe Msg**:  elle permet d'afficher des caractères à partir de font définie en 5*3 pixels. C'est ce qui va me permettre d'afficher la température du CPU.
{: .text-justify}
- **classe Sysdroid_uhhd**: elle va contenir toutes les méthodes qui permettent d'afficher les titres **P** - **R** - **D** pour **P**rocesseur - **R**AM - **D**ISK et lisser leur couleur en fonction du niveau d'occupation de ces derniers. Elle permet aussi d'afficher des barres de niveau sur 5 pixels qui changent de couleur et de luminosité en fonction du niveau (couleur lissée du vert au rouge en fonction du niveau qui varie de 0% à 100%). Une petite animation de démarrage et d'extinction est aussi ajoutée.
{: .text-justify}

Quand on exécute ce programme dans une console en tâche de fond "**python3 sysdroid_main.py &**", et qu'on regarde ce qu'il coûte en ressource via une commande "top": à peine 1.5% d'un CPU ! donc paris gagné: ce programme n'est pas du tout gourmand. On peut d'ailleurs voir que pas mal de processeurs sont totalement au repos puisque la première led verte ne s'allume même pas.
{: .text-justify}

Le programme utilise la bibliothèque [psutil](https://pypi.org/project/psutil/) pour récupérer les informations systèmes. Si cette erreur apparaît lors de l’exécution (et du coup rien ne se passe) :
{: .text-justify}
> ModuleNotFoundError: No module named 'psutil'

elle se règle très simplement avec cette commande qui va installer la bibliothèque manquante:
{: .text-justify}
> sudo pip3 install psutil

Démonstration du sysdroid Unicorn HAT HD en action, avec un coup de stress CPU qui fait monter en flèche l'occupation des 4 CPUs et de la température. Mon disque est volontairement saturé à 85% pour cette démo.
{: .text-justify}

{% include video id="Gn25LqN7EGs" provider="youtube" %}

> Voilà c'est le HAT que j'utilise désormais sur mon Raspberry pi3b+ qui me sert de serveur NAS (je ne le démarre que quand je veux lancer une synchronisation/sauvegarde de mes données). 
{: .text-justify}