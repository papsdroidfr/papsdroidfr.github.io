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
  teaser : "/assets/images/tutos/040MotorStepper/motor_300.jpg"

#SEO tags
title       : "Stepper motor"
image       : "/assets/images/tutos/040MotorStepper/motor_300.jpg"
description : "Une bibliothèque MicroPython pour contrôler un moteur pas à pas - Stepper Motor"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Stepper Motor"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "DEV" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-python" "électronique"
tags        : ["micro-python", "raspberry pico", "ESP32"]

gallery_schema:
  - url: /assets/images/tutos/040MotorStepper/fritzing.png
    image_path: "/assets/images/tutos/040MotorStepper/fritzing_800.png"
    title: "Schéma du projet - Fritzing"
---

![Stepper](/assets/images/tutos/040MotorStepper/motor_300.jpg){: .align-left}
Ce [module](https://github.com/papsdroidfr/MotorStepper) écrit en **MicroPython** permet de gérer un **moteur pas à pas** (stepper motor) avec un microcontrôleur de type Raspberry PICO, ESP32, ou SMT32. Il va permettre de définir les 4 ports GPIO qui vont commander le moteur, et de disposer de méthodes simples pour le mettre en rotation en contrôlant sa vitesse, son sens de rotation, et bien entendu de pouvoir l'arrêter sur un angle très précis, ou encore de pouvoir le mettre en veille.
{: .text-justify}

## Hardware

### Matériel nécessaire

Il vous faut:

- Un **Raspberry PICO**, ou bien un **ESP32** ou même un **SMT32** qui dispose d'au moins 4 ports GPIO
{: .text-justify}
- Un moteur pas à pas 12v de type **NEMA17** (personnellement j'ai pris un 17HE15)
{: .text-justify}
- Un **driver L298N** qui va permettre de convertir les instructions en 3.3v envoyées par le microcontrôleur en 12v vers les bobines du moteur pas à pas.
{: .text-justify}
- Une alimentation **DC 12v ou 5v**  ça fonctionne très bien en 5v: le moteur 12v aura moins de couple s'il est commandé en 5v, mais il reste parfaitement fonctionnel dans un cadre où il n'a pas beaucoup de résistance lorsqu'il doit tourner.
{: .text-justify}

{% include gallery id="gallery_schema" caption="Cliquez pour agrandir l'image." %}

> **Attention** une erreur très facile à faire est d'oublier de **relier toutes les masses entre elles**. Le fil de masse noir qui relie la masse de l'alimentation à celle du driver LN298N doit aussi être relié à la masse du microcontrôleur: c'est **indispensable**. (Dans mon exemple j'ai opté pour un Raspberry PICO)
{: .text-justify}

### Fonctionnement moteur

![Stepper](/assets/images/tutos/040MotorStepper/stepper_motor_300.jpg){: .align-left}
Il faut comprendre dans les grandes lignes comment fonctionne un moteur pas à pas, pour savoir si c'est adapté à votre projet. Un tel moteur est composé de **2 bobines A et B**, alimentée en 12v continu (5v ça marche aussi). Dans chaque bobine, on peut injecter du 12v par ses 2 entrées. Nommons ces 4 entrées A+, A-, B+, B-.
{: .text-justify}

- C'est pour cette raison qu'il faut **4 ports GPIO** disponibles pour pouvoir activer l'envoi de signaux 12v sur chaque entrée.
{: .text-justify}
- Je ne connais aucun microcontrôleur capable d'envoyer du 12v en sortie: c'est aussi pour cette raison qu'il faut un driver L298N qui va convertir les signaux (généralement en 3.3v) de sortie du microcontrôleur en tensions 12v.
{: .text-justify}

Un moteur pas à pas possède cette caractéristique de **décomposer une rotation complète 360° en n "steps" égaux**: on peut faire tourner le moteur d'un seul step, ce qui offre une **très grande précision** pour dire au moteur de tourner un angle précis, et ce **sans aucun asservissement** contrairement avec un moteur simple à courant continu. Les moteurs NEMA17 ont 200 steps, ce qui fait qu'on peut lui dire de tourner avec un angle multiple de 360/200 = 1.8° ! c'est très précis.
{: .text-justify}

Pour que le moteur puisse tourner, il faut envoyer dans les entrées A+, A-, B+, B- un cycle très précis de tensions 12v (ou 5v) et ce toutes les 5ms (ce délais d'attente est important sinon le rotor n'arrive pas à suivre le rythme). Ces tensions injectées dans les bobines ont pour conséquence de créer un champs électromagnétique qui va faire déplacer le rotor aimanté d'un "step", et ce dans un sens comme dans l'autre selon le cycle qu'on envoie dans les bobines.
{: .text-justify}

Les avantages d'un moteur pas à pas est donc de pouvoir le faire tourner dans un sens comme dans l'autre avec un angle très précis sans aucun asservissement. Et bien entendu en lui disant de tourner 200 fois et de recommencer sans arrêt il va tourner de fait en continu.
{: .text-justify}

Il y a tout de même quelques inconvénients avec les moteurs pas à pas:
{: .text-justify}

- ils fonctionnent par saccades (le rotor est déplacé d'un step et attends 5ms la prochaine instruction): cela en fait des moteurs qui génèrent beaucoup de vibrations: il faut les atténuer avec du matériel qui absorbe les vibrations.
{: .text-justify}

- Ils ne sont pas véloces: on ne peut pas envisager de tourner à 10 000 tours/minute avec un moteur qui se déplace par petits steps et attend 5ms entre deux.
{: .text-justify}

- Il faut l'actionner en respectant un cycle assez casse-tête et très rigoureux via 4 entrées de commande en 12v. Ce n'est aussi simple que d'activer un moteur à courant continu: une programmation informatique à l'aide d'un microcontrôleur est indispensable.
{: .text-justify}

> Si vous avez besoin d'un moteur capable de tourner dans un sens comme dans l'autre, en continu ou bien avec des angles précis sans asservissement mais uniquement quelques codages informatiques, et que vous n'avez pas besoin de vitesse excessive avec les rotations: les moteurs pas à pas sont idéaux, associé avec cette bibliothèque écrite en MicroPython c'est un jeux d'enfant de l'animer sans se soucier des cycles précis à lui envoyer.
{: .text-justify}

## Software


### Usage



### Exemple de code


### Pour aller plus loin

Dans un projet finalisé, il n'y aura pas besoin de faire tous ces print dans la log: il faudra instancier le ServerWeb avec l'option **debug=False**. Pour récupérer l'adresse du serveur WEB généré par le PICO il y a deux façons de faire:
{: .text-justify}

- Utiliser un petit écran OLED ou LCD pour y afficher l'adresse du serveur WEB.
{: .text-justify}
- Si vous utilisez un smartphone Android, l'application "**Net Analyser**" analyse et scanne tous les serveurs LAN actifs sur le réseau WIFI: on détecte ainsi facilement l'adresse du serveur WEB géré par le PICO.
{: .text-justify}