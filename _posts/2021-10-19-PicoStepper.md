---
#source: https://www.papsdroid.fr/post/stepperpico
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
  teaser : "/assets/images/tutos/034PicoStepper/picoStepper.jpg"

#SEO tags
title       : "Moteur pas à pas bipolaire et Raspberry PICO"
image       : "/assets/images/tutos/034PicoStepper/picoStepper.jpg"
description : "Afin de motoriser mon planétaire mécanique, je compte utiliser un moteur pas à pas bipolaire 12V commandé par un Raspberry PICO"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "PicoStepper"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pico" , "micro-python", "stepper-motor", "L298N"]

---
![PicoLeds](/assets/images/tutos/034PicoStepper/picoStepper.jpg){: .align-left} 
Afin de motoriser mon [#planétaire](https://papsdroidfr.github.io/tags/#planétaire) mécanique, je compte utiliser un **moteur pas à pas bipolaire 12V** commandé par un Raspberry PICO. Je ne vais pas détailler le fonctionnement des moteurs pas à pas ce serait trop long, il y a tout plein d'articles sur internet qui [l'expliquent très bien](https://www.alloschool.com/assets/documents/course-130/fonction-convertir-moteurs-pas-a-pas-cours.pdf). La raison pour laquelle je m'oriente vers un moteur pas à pas est que d'une part ils sont très faciles à mettre en œuvre contrairement à des moteurs à courant continu où il faut ajouter des capteurs ou une boucle d'asservissement, et surtout parce-que mon planétaire mécanique part d'un axe principal qui simule une semaine complète par rotation: avec un moteur pas à pas je peux simuler une journée = 1/7 de rotation, et compter précisément les jours qui passent sans aucun capteur.
{: .text-justify}
Le choix du moteur bipolaire par rapport à un monopolaire tient au fait qu'ils sont plus robustes et tournent plus vite que les monopolaires. J'avais un monopolaire à 5 fils 32 steps dans un kit: ils sont dotés d'un mécanisme de réduction interne 1/64 et au final il faut envoyer 32x64 = **plus de 2000 instructions** pour lui faire faire un tour complet: c'est vraiment très très lent. Je me suis donc orienté vers un **Bipolaire 200 steps** alimenté en 12v (Longruner 17HS13-0404S que l'on trouve pour environ 15€) : il n'a besoin que de 200 instructions (ou steps) pour faire un tour complet et ne consomme que 0.4A. Un step va le faire bouger d'une rotation de 1.8° environ.
{: .text-justify}

## Circuit électronique
![PicoLeds](/assets/images/tutos/034PicoStepper/Fritzing.png){: .align-center} 
C'est un driver **L298N** qui va envoyer du 12v (ou du 5v ça marche aussi, le moteur perd un peu en couple mais il fonctionne très bien sous 5v) dans les 2 bobines du moteur (bobine A et bobine B). Le PICO n'est pas assez puissant pour envoyer le courant dans les bobines: il va juste envoyer des instructions binaires (1/0) dans les 4 entrées In1, In2, In3, In4 du L298N qui va laisser passer le courant (12v ou 5v) dans les sorties OUT1, OUT2 (bobine A) et OUT3, OUT4 (bobine B). L'avantage d'alimenter ce moteur en 5v au lieu de 12V c'est que ça ne chauffe absolument pas, et le couple du moteur sous 5V est largement suffisant pour motoriser mon planétaire mécanique.
{: .text-justify}
Comme j'ai un peu galéré pour trouver les bons branchements, j'ai prévu dans la bibliothèque micropython un mode de test lent (1 step par 1/2 seconde) pour comprendre ce qu'il se passe. Les leds n'ont pas d'autre utilité que de vérifier les bonnes séquences sur les entrées In1 à In4, car pour faire tourner le moteur il faut envoyer des séquences bien précises. Je ne mettrait pas ces leds dans le montage final qui tournera à pleine vitesse.
{: .text-justify}
A noter que si vous testez le moteur avec une alimentation 5V: **c'est bien dans l'entrée 12V qu'il faut brancher le 5V**. En effet l'autre 5V sur le bornier est une sortie 5V réduite à partir de l'entrée 12V, elle permet par exemple à terme d'alimenter directement le PICO en 5v.
{: .text-justify}
Si vous vous trompez dans les branchements, le moteur ne va rien faire d'autre que vibrer au lieu de tourner. On comprend mieux ce qu'il se passe avec **le mode test lent**. Normalement vous avez 4 fils (noir, vert, rouge, bleu): le noir et le vert sont pour la bobine A, tandis que le rouge et le bleu pour la bobine B. Pour repérer les couples de bobine si vous avez récupéré un moteur sans ces fils de couleur (ou si vous ne voyez pas bien les couleurs) il suffit d'utiliser un ampèremètre: les 2 fils d'une même bobine se repèrent quand elles présentent une résistance faible de quelques dizaines de ohms (bien entendu ne pas faire cette manip avec le moteur branché!). **Attention les fils verts et rouge sont inversés** entre le moteur et la sortie de la nappe fournie (sur le moteur on voit noir, rouge, vert, bleu tandis qu'en sortie de la nappe on voit noir, vert, rouge, bleu): vérifiez bien que vous avez repéré les bons couples de bobine **sinon le moteur va vibrer sans jamais tourner**. J'ai bien galéré avec ça!
{: .text-justify}
Les entrées In1 à In4 seront respectivement pilotées par les sorties Pin14 à Pin17 du PICO comme le montre le schéma. J'ai réutilisé des câbles de couleur noir, vert, rouge et bleu pour faciliter la compréhension, mais si vous ne voyez pas bien les couleurs retenez simplement la légende qui apparaît en haut à droite du schéma c'est assez clair. N'oubliez pas de relier les masses entre elles (PICO, L289N et Alim 5 ou 12v).
{: .text-justify}

### Séquences pour faire tourner le moteur
![PicoLeds](/assets/images/tutos/034PicoStepper/StepperMotorDatasheet.png){: .align-center} 
Si l'on regarde le datasheet du moteur, il est expliqué qu'il faut utiliser la séquence ci-contre en 4 temps en boucle 1->2->3->4 et retour à 1 etc ... pour faire tourner le moteur: chaque phase faisant bouger le moteur d'un seul step. C'est donc très facile depuis le PICO d'envoyer en boucle ces séquences depuis les Pin14, 15, 16 et 17 dans un sens ou dans l'autre pour faire bouger le moteur dans un sens ou dans l'autre.: on remplace les + par des sorties à 1 et les - par des sorties à 0, et le tour est joué. Il faut respecter un temps d'attente **d'au moins 5ms** entre chaque changement de step, sinon le moteur sature et vibre sur place, avec risque de le détruire: les changements de positions ne peuvent pas se faire trop vite. Pour jouer sur la vitesse de rotation il suffit simplement d'augmenter ce délais d'attente: avec 10ms il tournera 2 fois plus lentement qu'avec 5ms d'attente entre chaque step. Bien entendu ces moteurs avec 5ms d'attente minimum entre chaque step ne sont pas du tout conçus pour tourner à 6000tr/mn, et heureusement je n'ai pas besoin d'une telle vitesse pour animer mon planétaire mécanique.
{: .text-justify}

## Bibliothèque micropython
Vous trouverez la bibliothèque dans le dossier **/micropython** du [Github du projet](https://github.com/papsdroidfr/planetaire). Il s'agit du programme **bipolarStepper.py** à déposer à la racine de votre PICO. Si vous utilisez pour la première fois un Raspberry PICO [cet article](https://papsdroidfr.github.io/configuration/pico/) vous explique comment le configurer. 
{: .text-justify}
Dans cette bibliothèque vous avez une classe **BipolarStepper()** qui s'initialise avec les paramètres:
{: .text-justify}
* **speed** = 'high', 'medium','low' ou 'test'. Si vous voulez comprendre ce qu'il se passe utilisez speed='test'. Par défaut speed est initialisé à 'high'
* **direction** = 'forward' ou 'backward' pour influencer le sens de rotation du moteur. ('forward' par défaut)
* **steps360** = nombre de steps du moteurs pour faire une rotations complète 360°, par défaut 200.

### Quelques exemple d'instanciation en micropython
```python
from bipolarStepper import BipolarStepper

#speed medium, direction forward
bp1 = BipolarStepper(speed='medium') 

#speed test, direction backward
bp2 = BipolarStepper(speed='test', direction = 'backward')

# highest speed, direction forward
bp3 = BipolarStepper()
```
>A noter que lors de l'instanciation d'un objet BipolarStepper, le moteur se positionne immédiatement dans la 1ère position du cycle 1->2->3->4 puis attend 0,5s. Vous avez donc 1 chance sur 4 de voir le moteur bouger d'un step à ce moment là: c'est normal.
{: .text-justify}
Les méthodes suivantes sont définies pour contrôler le moteur:
{: .text-justify}
* **set_speed(speed)**: modifie la vitesse parmi 'high', 'medium', 'low,' 'test' (si vous mettez n'importe quoi d'autre il va utiliser 'high' à la place)
* **set_direction(direction)**: modifie la direction entre 'forward' et 'backward' ('forward' sera utilisé à défaut)
* **next_steps(nsteps)**: fait bouger le moteur de nsteps (par défaut 1) dans la direction et à la vitesse courante du moteur.
* **next_angle(angle)**: fait bouger le moteur d'un certain angle (exprimé en degrés), dans la direction et à la vitesse courante du moteur.
* **split_steps(nsplits)**: renvoie une liste: décompose steps360 en nsplits phases équivalentes. C'est cette méthode qui va me permettre de décomposer les 200 steps en une série de 7 mouvements pour simuler chaque jour de la semaine. Si la division de steps360/nsplits n'est pas entière, je répartis le surplus de steps restants à la fin.

### Quelques exemples, la dernière boucle permet de faire bouger le moteur 7 fois pour faire un tour complet.
```python
from bipolarStepper import BipolarStepper
import utime

bp = BipolarStepper(speed='medium')   

# move 100 steps forward, speed medium 
bp.next_steps(100) 
utime.sleep(0.5)

# move 50 steps backward highest speed
bp.set_direction('backward')            
bp.set_speed('high')            
bp.next_steps(50)                
utime.sleep(0.5)

#split into 7 phases: returns [28, 28, 28, 29, 29, 29, 29]
step_days = bp.split_steps(7)
for s in step_days:
    bp.next_steps(s) #move the motor nbsteps = s
    utime.sleep(0.2)
```
>Voilà cette bibliothèque fait maison me convient pour animer mon planétaire mécanique, pas besoin d'en faire plus. J'ai mes méthodes pour contrôler la vitesse, le sens et décomposer un tour complet en 7 mouvements égaux à 1 step près (différence imperceptible).
{: .text-justify}