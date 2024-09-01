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
tags        : ["micro-python", "raspberry pico", "ESP32", 'stepper-motor', 'L298N']

gallery_schema:
  - url: /assets/images/tutos/040MotorStepper/fritzing.png
    image_path: "/assets/images/tutos/040MotorStepper/fritzing_800.png"
    title: "Schéma du projet - Fritzing"
---

![Stepper](/assets/images/tutos/040MotorStepper/motor_300.jpg){: .align-left}
Ce [module](https://github.com/papsdroidfr/StepperMotor) écrit en **MicroPython** permet de gérer un **moteur pas à pas** (stepper motor) avec un microcontrôleur de type Raspberry PICO, ESP32, ou SMT32. Il va permettre de définir les 4 ports GPIO qui vont commander le moteur, et de disposer de méthodes simples pour le mettre en rotation en contrôlant sa vitesse, son sens de rotation, et bien entendu de pouvoir l'arrêter sur un angle très précis, ou encore de pouvoir le mettre en veille.
{: .text-justify}

## Hardware

### Matériel nécessaire

Il vous faut:

- Un **Raspberry PICO**, ou bien un **ESP32** ou même un **SMT32** qui dispose d'au moins 4 ports GPIO
{: .text-justify}
- Un moteur pas à pas 12v de type **NEMA17** (personnellement j'ai pris un modèle 17HE15-1504S)
{: .text-justify}
- Un **driver L298N** qui va permettre de convertir les instructions en 3.3v envoyées par le microcontrôleur en 12v vers les bobines du moteur pas à pas.
{: .text-justify}
- Une alimentation **DC 12v ou 5v**  ça fonctionne très bien en 5v: le moteur 12v aura moins de couple s'il est commandé en 5v, mais il reste parfaitement fonctionnel dans un cadre où il n'a pas beaucoup de résistance lorsqu'il doit tourner.
{: .text-justify}
- Des petits câbles de raccord Dupont, en **respectant rigoureusement le schéma ci-dessous**. Si le moteur "vibre" au lieu de tourner correctement, c'est que le branchement des câbles n'est pas correct.
{: .text-justify}

{% include gallery id="gallery_schema" caption="Cliquez pour agrandir l'image." %}

> **Attention** une erreur très facile à faire est d'oublier de **relier toutes les masses entre elles**. Le fil de masse noir qui relie la masse de l'alimentation à celle du driver LN298N doit aussi être relié à la masse du microcontrôleur: c'est **indispensable**. (Dans mon exemple j'ai opté pour un Raspberry PICO)
{: .text-justify}

### Fonctionnement moteur

![Stepper](/assets/images/tutos/040MotorStepper/stepper_ia_300.jpg){: .align-left}
Il faut comprendre dans les grandes lignes comment fonctionne un moteur pas à pas, pour savoir si c'est adapté à votre projet. Un tel moteur est composé de **2 bobines A et B**, alimentée en 12v continu (5v ça marche aussi). Dans chaque bobine, on peut injecter du 12v par ses 2 entrées. Nommons ces 4 entrées A+, A-, B+, B-.
{: .text-justify}
- C'est pour cette raison qu'il faut **4 ports GPIO** disponibles pour pouvoir activer l'envoi de signaux 12v sur chaque entrée.
{: .text-justify}
- Je ne connais aucun microcontrôleur capable d'envoyer du 12v en sortie: c'est aussi pour cette raison qu'il faut un driver L298N qui va convertir les signaux (généralement en 3.3v) de sortie du microcontrôleur en tensions 12v.
{: .text-justify}
Un moteur pas à pas possède cette caractéristique de **décomposer une rotation complète 360° en n "steps" égaux**: on peut faire tourner le moteur d'un seul step, ce qui offre une **très grande précision** pour dire au moteur de tourner un angle précis, et ce **sans aucun asservissement** contrairement avec un moteur simple à courant continu. Les moteurs NEMA17 ont 200 steps, ce qui fait qu'on peut lui demander de tourner avec un angle multiple de 360/200 = 1.8° ! c'est très précis.
{: .text-justify}

Pour que le moteur puisse tourner, il faut envoyer dans les entrées A+, A-, B+, B- un cycle précis de tensions 12v (ou 5v) et ce toutes les 5ms (ce délais d'attente est important sinon le rotor n'arrive pas à suivre le rythme). Ces tensions injectées dans les bobines ont pour conséquence de créer un champs électromagnétique qui va faire déplacer le rotor aimanté d'un "step", et ce dans un sens comme dans l'autre selon le cycle qu'on envoie dans les bobines.
{: .text-justify}

**L'avantage d'un moteur pas à pas** est donc de pouvoir le faire tourner dans un sens comme dans l'autre avec un angle précis sans aucun asservissement. Et bien entendu en lui disant de tourner 200 fois et de recommencer sans arrêt, il va tourner de fait en continu.
{: .text-justify}

Il y a tout de même **quelques inconvénients** avec les moteurs pas à pas:
{: .text-justify}

- **Ils fonctionnent par saccades** (le rotor est déplacé d'un step et attends 5ms la prochaine instruction): cela en fait des moteurs qui génèrent beaucoup de vibrations: il faut les atténuer avec du matériel qui absorbe les vibrations.
{: .text-justify}

- **Ils ne sont pas rapides**: on ne peut pas envisager de tourner à 10 000 tours/minute avec un moteur qui se déplace par petits steps et attend 5ms entre deux.
{: .text-justify}

- Il faut envoyer **un cycle assez casse-tête** via 4 entrées de commande en 12v. Ce n'est pas aussi simple que d'activer un moteur à courant continu: une programmation informatique à l'aide d'un microcontrôleur est indispensable.
{: .text-justify}

> Si vous avez besoin d'un moteur capable de tourner dans un sens comme dans l'autre, en continu ou bien avec des angles précis sans asservissement mais uniquement quelques codages informatiques, et que vous n'avez pas besoin de vitesse excessive avec les rotations: les moteurs pas à pas sont idéaux, associé avec cette bibliothèque écrite en MicroPython c'est un jeux d'enfant de l'animer sans se soucier des cycles précis à lui envoyer.
{: .text-justify}

## Software

### Installation bibliothèque

Installez bien la dernière version MicroPython sur votre microcontrôleur. Cet [article](https://papsdroidfr.github.io/configuration/pico/) détaille comment s'y prendre avec un Raspberry PICO.
{: .text-justify}

Tout le code micropython est à récupérer sur le [Github du projet](https://github.com/papsdroidfr/StepperMotor) dans la section **micropython**.
{: .text-justify}

Il faut ensuite importer tout le module stepper à la racine de son PICO: **créer le dossier /stepper** à la racine du microcontrôleur et y importer tous les programmes du module:
{: .text-justify}
- **\_\_init\_\_.py**: initialisation du module
{: .text-justify}
- **pibolar.py**: classe de gestion du moteur
{: .text-justify}
- **pinout.py**: classe d'association des ports GPIO vers les entrées IN du driver LN298.
{: .text-justify}

### Usage

Ce programme montre quelques exemples de commande du moteur.
{: .text-justify}
- Une rotation complète 200 pas, à la vitesse "medium.
{: .text-justify}
- Un demi-tour dans l'autre sens, à pleine vitesse "high"
{: .text-justify}
- 4 tours consécutifs à 90°
{: .text-justify}
- Calcul d'un split de 200 pas en 7 tranches.
{: .text-justify}

**2 directions** sont à utiliser: 'backward'(par défaut) ou 'forward'.
{: .text-justify}

**4 vitesses** sont proposée: speed = "high"(par défaut), "medium", "low", ou "test". La vitesse speed='test' permet de voir le moteur tourner step par step à très basse vitesse (1/2 secondes entre chaque): vous pouvez ainsi constater sur une dizaine de steps qu'il tourne bien toujours dans le même sens. Si ce n'est pas le cas, c'est que les branchements sont à revoir (et du coup il vibre au lieu de tourner à grande vitesse).
{: .text-justify}


```python
import utime
from stepper.bipolar import BipolarStepper

print('test module stepper')
bp = BipolarStepper(speed='medium')   

print('move 200 steps forward, speed medium') 
bp.next_steps(200)
bp.sleep()
utime.sleep(0.5)

print('move 100 steps backward highest speed')
bp.set_direction('backward')            
bp.set_speed('high')            
bp.next_steps(100)
bp.sleep()
utime.sleep(0.5)
    
print('move 4 times to 90°')
for _ in range(4):
    bp.next_angle(90)
    bp.sleep()
    utime.sleep(0.5)

print(f"split {bp.steps360} steps in 7: {bp.split_steps(7)}")

bp.sleep()
print('bye')
```

> Si le moteur vibre au lieu de tourner correctement avec ce programme test, c'est que vos branchements sont à revoir.
{: .text-justify}

Si vous voulez utiliser **d'autres GPIO** que ceux utilisés par défaut (GPIO_17:in1, GPIO_16:in2, GPIO_15:in3, GPIO_14:in4), il suffit de passer les paramètres pin1=mon_gpio1, pin2=mon_gpio2, pin3=mon_gpio3 et pin4=mon_gpio4 lors de l'instanciation d'un objet bp=BipolarStepper(), exemple ci-dessous avec les GPIO 10 à 13:
{: .text-justify}

```python
bp = BipolarStepper(speed='medium', in1=10, in2=11, in3=12, in4=13) 
```

Enfin si votre moteur pas à pas ne fait pas 200 steps, utilisez la variable **steps360** (par défaut initialisée à 200) pour le préciser.


Notez bien que la **méthode sleep()** est très importante pour **mettre le moteur en veille** et ne plus envoyer de tension dans les 4 fils de commande. Dans ce cas, le moteur est relâché et ne consomme plus aucun courant. On peut le faire tourner à la main puisque le rotor n'est plus maintenu en place par un champs électromagnétique. Attention s'il subi un couple de contrainte trop importante il va se mettre à tourner tout seul pour retrouver son équilibre sans contrainte ...
{: .text-justify}

A l'inverse si vous ne mettez pas le moteur en veille avec cette méthode .sleep(), 2 des 4 fils restent sous tension et vont maintenir avec force le rotor en place: c'est quasiment impossible de le faire bouger à la main. Mais du 12v est envoyé en continu dans les bobines: **le radiateur du LN298 va se mettre à pas mal chauffer** !
{: .text-justify}


### Pour aller plus loin

- Vous pouvez vous amuser à brancher 4 leds (avec leur résistance) en // des 4 sortie GPIO et utiliser la vitesse 'test' pour les voir s'allumer selon le cycle de séquence programmé. Les autres vitesses vont trop vite pour nos yeux: vous aurez l'impression que les 4 leds sont tout le temps allumées.
{: .text-justify}

- Si vous voulez commander **plusieurs moteurs** avec le même microcontrôleur il suffit d'instancier plusieurs objets BipolarStepper() mais avec des GPIO différents pour chaque moteur. Il faut 4 GPIO de libre par moteur.
{: .text-justify}