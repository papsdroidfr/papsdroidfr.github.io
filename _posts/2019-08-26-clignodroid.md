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
  teaser : "/assets/images/tutos/001clignodroid/20190805_220333_300.jpg"

#SEO tags
title       : "Clignodroid"
image       : "/assets/images/tutos/001clignodroid/20190805_220333_300.jpg"
description : "Un jeux rigolo basé sur le principe du jeux pour enfants «Simon»"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Clignodroid"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pzero" ,  "python3", "électronique"]

gallery1:
  - image_path: "/assets/images/tutos/001clignodroid/20190809_151447_web.jpg"
    title: "PCB dessous"
  - image_path: "/assets/images/tutos/001clignodroid/20190809_151506_web.jpg"
    title: "PCB dessus"
gallery2:
  - image_path: "/assets/images/tutos/001clignodroid/20190806_212914_web.jpg"
    title: "vue dessous"
  - image_path: "/assets/images/tutos/001clignodroid/20190809_152633_web.jpg"
    title: "vue côté"
---

![clignodroid](/assets/images/tutos/001clignodroid/20190805_220333_300.jpg){: .align-left} 
Ce tutoriel représente un jeux rigolo à base de **Raspberry pi zero**, basé sur le principe du jeux pour enfants « Simon » 
<!--more-->
Il faut répéter une séquence de leds qui s’allument au hasard, en commençant par 1 led, puis 2, 3 ... jusqu’à ce que le joueur se trompe. Dans ma version clignodroid, la séquence de leds est aléatoire à chaque fois, contrairement au jeux Simon qui augmente la séquence précédente d'une nouvelle led tant qu'on ne se trompe pas: ceci rend considérablement difficile l'effort de mémorisation, s'il y en a un qui arrive à dépasser un score de 9 qu'il me prévienne! Le jeux mémorise le meilleur score (tous joueurs confondus). Chaque led est associée à un bouton poussoir de même couleur pour que le joueur entre la séquence qu’il vient de voir. Un bouton poussoir supplémentaire noir "Off" permet d’éteindre proprement tout le système. 
{: .text-justify}

Le jeux est équipé d’un afficheur LCD de 2 lignes de 16 caractères pour afficher :

- un message d’initialisation
- une invite pour commencer la partie (avec activation du buzzer ou pas)
- affichage du niveau atteint par le joueur et score à battre
- message d’extinction du système

## Synoptique du jeux
![synoptique](/assets/images/tutos/001clignodroid/synoptique.png)

## Matériel nécessaire
![Matériel](/assets/images/tutos/001clignodroid/matos1.png)
![Matériel](/assets/images/tutos/001clignodroid/matos2.png)

## Configuration du système

- Installer une version Raspbian strech ligth sur le raspberry pi, [sans écran ni clavier](https://raspberry-pi.fr/raspberry-pi-sans-ecran-sans-clavier/)
- Activer [le bus  I2C](https://projetsdiy.fr/activer-bus-i2c-raspberry-pi-3-pi-zero-w/) du raspberry pi, afin de pouvoir gérer l’afficheur LCD via port I2C
> le raspberry pi est alors prêt pour exécuter le programme écrit en python 3, qui va contrôler le circuit imprimé via le port GPIO. 
 
## Circuit électronique

Conçu sous KICAD, je fourni les fichiers GERBER qui permettent de le faire imprimer chez un pro.
![circuit](/assets/images/tutos/001clignodroid/Capture du 2019-07-10 19-08-10.png)

Je n'ai absolument rien inventé mais juste mis en pratique mes apprentissages de tutoriels très simples de gestion de leds, de commande de boutons poussoirs, de commande d'un buzzer actif, et d'affichage sur un écran LCD via une interface I2C. Le net regorge de tutoriels en la matière je vous encourage à les pratiquer si vous êtes novice.

- Chaque **led** (une verte, une bleue, une jaune et une rouge) est reliée via une résistance 220 ohms à un port GPIO dédié en mode OUT: la Led s'allume quand le port est activé en HIGH et s’éteint s'il est basculé en LOW.
- Chaque **bouton poussoir** (1 pour commande chaque led + 1 pour la commande d'arrêt) est relié avec 2 résistances de 10 kilo ohms, connecté à un port GPIO dédié en mode IN. Quand il n'est pas activé, la tension qui arrive au port traverse les 2 résistances reliées au +3.3V: le signal est considérée comme HIGH. Quand il est activé, la résistance connectée au port GPIO se retrouve alors reliée à la masse (l'autre résistance évite un court-circuit entre la masse et le +3.3v !): le signal lu est alors LOW.
- Le **Buzzer** est commandé par un transistor NPN dont la base est alimenté par un port GPIO en mode OUT qui va envoyer un signal carré d'une certaine fréquence (une fréquence pour chaque led) afin de jouer une note de musique.
- Enfin **l'afficheur LCD** relié à son module I2C est simplement relié aux 2 ports de communication I2C du Rapsperry pi + la masse et l'alimentation +3.3v: il ne faut que 4 broches pour afficher des messages sur un LCD avec son module I2C déjà soudé.

{% include figure image_path="/assets/images/tutos/001clignodroid/20190710_190111_web.jpg" caption="test du circuit sur une breadboard" %}

### Soudure des composants

Le circuit imprimé a été conçu sous KICAD: les fichiers GERBER fournis permettent de le faire imprimer chez un pro. 

{% include gallery id="gallery1" caption="soudure des composants" %}

Commencer par souder la nappe 2*20pin femelle à positionner sous le PCB : le Raspberry pi va se loger sous le PCB qui sera surélevé par des entretoises.  

> Ne pas hésiter à utiliser une loupe pour souder les nombreuses pattes qui sont proches.

Souder ensuite les résistances 220Ω verticalement, (R1 R2 R3 R4 en haut à gauche, bleues sur l’image). Ces résistances sont associées aux Leds

Souder ensuite les résistances 10kΩ verticalement sur la droite (R5 à R14). **/!\ Attention à la rangée du bas**: le sens des résistances est important. Si elles sont soudées dans l’autre sens, le schéma électrique n’est plus respecté et ça ne fonctionnera pas.

Souder ensuite la résistance verticale 1kΩ (R15) au dessus du buzzer ainsi que le transistor juste en dessous. Ne pas hésiter à utiliser une loupe pour souder les 3 pattes très proches du transistor NPN… **Attention à bien le souder dans le bon sens** (méplat vers le buzzer,comme sur la photo), sinon le buzzer ne fonctionnera pas.

Souder le buzzer: **le sens est aussi important**: patte + soudée vers le bas.

Souder les Leds en prenant soin de disposer les clips avant de souder (sinon impossible de les mettre une fois les Leds soudées), et de respecter les couleurs (verte, bleue, jaune, rouge). **Le sens est important**: la patte la plus longue doit être soudée dans le trou rond de droite (qui lui est relié à une résistance 220Ω). Le trou « carré » est quand à lui relié à la masse. Si elles sont soudées dans l’autre sens : elles ne s’allumeront jamais.

Souder tous 5 les boutons poussoir, le sens n’a pas d’importance.

Souder le connecteur mâle 4 pin coudé en haut à gauche.

### Assemblage du circuit

{% include gallery id="gallery2" caption="assemblage" %}

Connecter le Raspberry pi directement sur la barrette 2*20pin située sous le PCB. Disposer les 7 entretoises 15mm en bas, et les 4 entretoises 20mm au dessus (support du LCD). Brancher la nappe souple (connecteurs coudés) vers port I2C du LCD: **attention au sens** (GND en haut, puis VCC, SDA et SCL).

A noter que l’utilisation d’une barrette 2*20pin **bas profil (5mm de hauteur)** est importante, sinon les entretoises de 15mm en support ne seraient pas assez hautes pour éviter que le Raspi ne traîne par terre. De même le LCD doit être accompagné de son module I2C soudé par dessous le LCD . Il y a des composants qui prennent de la place + ceux du PCB : il faut à minima des entretoises de 20mm pour surélever correctement le LCD.


## Programmes Python

### détail des programmes

Le programme est écrit en Python3, orienté objet.

Plusieurs modules sont nécessaires:

gestion du LCD avec port I2C:(programmes récupérés depuis un tutoriel d’affichage LCD avec un raspberry pi)
- PCF8574.py
- Adafruit_LCD1602.py


Programme ClignoDroid (orienté objet):
- clignoDroid.py:  programme principal
- clignodroid_buton.py: classe de gestion des boutons poussoirs
- clignodroid_buz.py: classe de gestion du buzzer
- clignodroid_lcd.py: classe de gestion de l’affichage LCD
- clignodroid_led.py: classe de gestion des Leds


Fichier de configuration :
- score.txt : simple fichier texte dans lequel le meilleur score est stocké (initialisé à 3)


{% include figure image_path="/assets/images/tutos/001clignodroid/Capture du 2019-08-09 17-34-20.png" caption="Bien s’assurer que les programmes sont recopiés avec des droits en lecture et en écriture." %}

 
### Installation des programmes

Tous les programmes (les 8 fichiers *.py) ainsi que le fichier score.txt doivent être ajoutés dans un dossier /home/pi/ClignoDroid/

(Attention avec les MAJUSCULES et minuscules: elles sont importantes)

Ensuite pour que le programme principal s’exécute au démarrage du Raspberri pi, il faut entrer les commandes suivantes en mode console (via une connection SSH, comme lors de l’installation du système sans écran) :

→ **sudo nano /etc/rc.local**

Ajouter juste avant la ligne exit 0 ceci:

→ **python "/home/pi/ClignoDroid/clignoDroid.py" &**

(le & à la fin est important pour que le programme puisse s’exécuter en tâche de fond et ne bloque pas le systèlme)

En branchant le raspberry pi: le système démarre (ça prend du temps, c’est normal ….) et lorsque le programme s’exécute il commence par faire clignoter toutes les Leds, avant de demander si on commence la partie: c’est parti!!!  

Si le joueur appuie sur le bouton noir « OFF » cela va éteindre proprement le raspberry pi (équivalent à une commande sudo poweroff sous SSH) : pour recommencer une partie il faut alors débrancher et rebrancher le système. Il n’est pas du tout recommandé de débrancher « sauvagement » le raspberry pi pendant qu’il fonctionne: à terme la SD carte serait corrompue voire détruite et plus rien ne fonctionnerait.

### Tests

Il faut se connecter au Raspberry pi via une connexion SSH depuis un ordinateur.  

il suffit alors d’exécuter le programme principal et des « print » logés dans les programmes permettent de voir ce qu’il se passe et éventuellement des messages d’erreurs s’il y en a.

Source d’erreur fréquentes:   

- essayer d’exécuter le programme avec un mauvais branchement sur port I2C du LCD va faire sortir le programme en exception puisqu'il n’arrivera pas à communiquer avec le port I2C pour afficher les messages au joueur.
- mettre les fichiers dans un autre répertoire que celui indiqué: dans ce cas il faut penser à adapter la ligne de commande d’exécution automatique
- oublier de déposer le fichier score.txt ou le nommer autrement.
- pas de droits en écriture sur le fichier score.txt


### Télécharger le code 

c'est ici sur ma page [Github](https://github.com/papsdroidfr/raspberry_clignodroid)

> Voilà tout est dit, vous pouvez dans un premier monter le projet en utilisant une Breadboard ce n'est pas trop compliqué comme branchements à faire. Comme j'ai commandé le PCB en 5 exemplaires, vous pouvez me contacter pour que je vous en fasse parvenir un sans aucun bénéfice de ma part: ça vous évitera d'en commander aussi 5 exemplaires dont 4 qui ne servent à rien ...