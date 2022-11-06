---
#source: https://www.papsdroid.fr/post/motorisation-planetaire
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
  teaser : "/assets/images/tutos/033Planetaire/boitier3D.png"

#SEO tags
title       : "Planétaire: motorisation"
image       : "/assets/images/tutos/033Planetaire/boitier3D.png"
description : "Dans cet article, je détaille les plans du boîtier de commande de mon #planétaire mécanique"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Motorisation"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["planétaire", "raspberry pico", "micro-pyhton", "électronique", "L298N", "stepper-motor", "écran LCD" ]

gallery1:
  - image_path: "/assets/images/tutos/033Planetaire/boitier3D-1.png"
  - image_path: "/assets/images/tutos/033Planetaire/boitier3D-2.png"
  - image_path: "/assets/images/tutos/033Planetaire/boitier3D-3.png"

---
![Planétaire](/assets/images/tutos/033Planetaire/boitier3D.png){: .align-left}
Dans cet article, je détaille les plans du boîtier de commande de mon [#planétaire](https://papsdroidfr.github.io/tags/#planétaire) mécanique. Un [Raspberry Pico](https://papsdroidfr.github.io/configuration/pico/) contrôle un [moteur bipolaire](https://www.papsdroid.fr/post/stepperpico) pas à pas 200 steps, avec [un écran LCD](https://papsdroidfr.github.io/tutoriels/picoLCD/) pour afficher quelques informations et des boutons poussoirs qui permettent le contrôle du système. Le liens ci-dessus vous renverrons vers des tutoriels d'initiations.
{: .text-justify}

## Fonctionnalités
Elles sont basiques, il s'agit de pouvoir:
* Modifier le mode de fonctionnement du planétaire: jour, semaine, mois, infini. Il correspond au nombre de jours que le système va simuler (mode jour = 1 jour, mode semaine = 7 jours etc ...).
{: .text-justify}
* Lancer/arrêter la révolution des planètes selon le mode choisi.
{: .text-justify}
* Voir le nombre de jours qui défilent au fur et à mesure que les planètes tournent.
{: .text-justify}
* Réinitialiser le système.
{% include figure image_path="/assets/images/tutos/033Planetaire/LCDmotor1.png" caption="Initialisation du système" %}
![Planétaire](/assets/images/tutos/033Planetaire/LCDmotor2.png){: .align-left}
On retrouve toutes ces informations sur l'écran LCD. Les 3 boutons poussoirs en façade servent à réinitialiser le système, changer le mode, et arrêt/relancer l'animation. Dans le mode "infini" le système planétaire ne s'arrêtera que lorsque l’utilisateur appuie sur le bouton d'arrêt/relance. Dans les autres modes, le système s'arrête tout seul lorsqu'il a atteint le nombre de jour défini. C'est rigolo de voir à quel point il ne se passe pas grand chose en une journée.
{: .text-justify}

## Circuit électronique
### Matériel nécessaire:
* 1 Raspberry PICO avec pin header soudés
* 3 petits boutons poussoirs 6mm
* 1 condensateur céramique 100nf
* 1 LCD 1602 avec backpack I2C
* 1 diode Schottky SR560
* 1 prise jack DC 5v 
* 1 alimentation 5V 2A
* 1 Driver L298N
* 1 moteur bipolaire 200 steps 12V (il fonctionne parfaitement sous 5v)
* quelques câbles pour relier le PICO au LCD (4 câbles), et L289N (6 câbles)

Optionnel:
* 1 PCB à imprimer, les fichiers GERBER zippés sont dans le dossier **/GERBER** du [Github du projet](https://github.com/papsdroidfr/planetaire).
* 3 entretoises M2.5 20mm en nylon (pour maintenir le LCD sur le PCB) (3 suffisent)
* 1 pin header coudé 4 pas 2.54mm (pour relier le LCD)
* 1 pin header droit 6 pas 2.54mm (pour relier le L298N)

Si vous préférez vous passer du PCB, le schéma à réaliser est le suivant:
{: .text-justify}
![Planétaire](/assets/images/tutos/033Planetaire/fritzing.png){: .align-center}
le Raspberry PICO commande le moteur pas à pas via un driver L298N, vous pouvez vous reporter à [cet article](https://www.papsdroid.fr/post/stepperpico) détaillé. Quand au LCD idem je détaille [ici](https://papsdroidfr.github.io/tutoriels/picoLCD/) comment écrire des textes et des caractères spéciaux à l'aide du Raspberry PICO.
{: .text-justify}

### Circuit imprimé
![Planétaire](/assets/images/tutos/033Planetaire/PCB-1.png){: .align-left}
Les composants sont faciles à souder sur le PCB bien sérigraphié. Commencez par souder tout ce qui est au dessus: boutons poussoirs, diode Schottky (**attention au sens**), condensateur céramique, les pins headers pour relier le LCD et le L298N (j'ai opté pour un pin header coudé pour le LCD, et un droit pour le L298N), puis la prise Jack. Le Raspberry PICO quand à lui se dispose **SOUS** le PCB: il faut donc faire les soudures par dessus. **Attention au sens**: la sortie USB du PICO est dans la même orientation que la prise Jack. 
{: .text-justify}
>Si vous soudez la Schottky à l'envers: le système ne sera pas alimenté. Si vous soudez le PICO dans l'autre sens: tout va partir en fumée à l'allumage ...
{: .text-justify}

## Scripts micropython
Les scripts micropython sont à récupérer dans le dossier **/micropython** du [Github du projet](https://github.com/papsdroidfr/planetaire). Recopiez les tous à la racine du PICO. Si vous voulez un démarrage automatique du système dès le branchement: il faut renommer le script **mainSolarPico.py** en **main.py**
{: .text-justify}

## Boîtier imprimé 3D
![Planétaire](/assets/images/tutos/033Planetaire/boitier3D.png){: .align-left}
Si vous imprimez le PCB avec le LCD disposé dessus avec 3 entretoises de 20mm, vous pouvez imprimer ce boîtier fait sur mesure. Les STL sont à récupérer dans le dossier **/STLPlanetaire/MOTOR/** du [Github du projet](https://github.com/papsdroidfr/planetaire). Le boîtier est composé de deux parties haute et basses à assembler qui prendront respectivement 2h50 et 6h20 d'impression en 0.2mm sans support nécessaire.
{: .text-justify}
{% include gallery id="gallery1" caption="" %}

Le PCB avec le LCD et le L298N trouveront naturellement leur place dans la partie basse du boîtier. Disposez les câbles de sorte à ne pas gêner les boutons poussoirs. les 4 fils du Stepper Motor peuvent sortir par une petite fente prévue à cet effet.
{: .text-justify}
![Planétaire](/assets/images/tutos/033Planetaire/motor-assemblage1.jpg){: .align-center}

![Planétaire](/assets/images/tutos/033Planetaire/motor-assemblage2.jpg){: .align-left}
Le couvercle vient se fixer dans les rainures sur le côté: un petit "click!" signifie qu'il est fixé . Si vous avez bien renommé le script principal en **main.py**, le système s'initialise dès que vous branchez la prise Jack 5V. Le fait d'alimenter le moteur en 5v au lieu de 12v fait que le driver L289N ne chauffe absolument pas: je l'ai laissé fonctionné plusieurs heures sans arrêt: le radiateur reste à température ambiante. **Attention** si vous mettez du 12V pour voir, le Raspberry PICO et le LCD n'y survivront pas !
{: .text-justify}

>Prochain article: je vais détailler les plans de montage complet du planétaire mécanique imprimé 3D, tous les fichiers STL sont d'ores et déjà sur le [Github du projet](https://github.com/papsdroidfr/planetaire).
{: .text-justify}

![Planétaire](/assets/images/tutos/033Planetaire/motor-assemblage3.jpg){: .align-center}