---
#source: https://www.papsdroid.fr/post/pimometre
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
  teaser : "/assets/images/tutos/021Pimometre/pimometre-300.jpg"

#SEO tags
title       : "Pimomètre"
image       : "/assets/images/tutos/021Pimometre/pimometre-300.jpg"
description : "Création d'une mini station météo de salon pilotée par un Raspberry pi zero"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Pimometre"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "tutoriels" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pzero" , "python3", "électronique", "écran LCD", "DHT22" ]

gallery1:
  - image_path: "/assets/images/tutos/021Pimometre/circuit1.jpg"
  - image_path: "/assets/images/tutos/021Pimometre/circuit2.jpg"
  - image_path: "/assets/images/tutos/021Pimometre/circuit3.jpg"

gallery2:
  - image_path: "/assets/images/tutos/021Pimometre/ecran01.png"
  - image_path: "/assets/images/tutos/021Pimometre/ecran02.png"
  - image_path: "/assets/images/tutos/021Pimometre/ecran03.png"

gallery3:
  - image_path: "/assets/images/tutos/021Pimometre/boitier3D1.png"
  - image_path: "/assets/images/tutos/021Pimometre/boitier3D2.png"
  - image_path: "/assets/images/tutos/021Pimometre/boitier3D3.png"
---
![Pimometre](/assets/images/tutos/021Pimometre/pimometre-300.jpg){: .align-left}
Ce tutoriel concerne la création d'une mini station météo de salon pilotée par un Raspberry piZERO. Elle affiche sur un écran LCD la température et humidité ambiante d'un capteur DHT22, ainsi que les prévisions météo locale des 12 prochaines heures via une connexion à une API de météo. Un boîtier sur mesure (optionnel) est proposé en impression 3D si vous êtes équipé d'une imprimante 3D.
{: .text-justify}

## Matériel nécessaire
![Pimometre](/assets/images/tutos/021Pimometre/materiel.jpg)
- 1 Raspberry pi Zero WH
{: .text-justify}
- 1 capteur température et humidité: DHT22 (précision T°: 0.5°C, précision humidité: 5%)
{: .text-justify}
- 1 résistance 4,7 k ohms (généralement fournie avec le capteur DHT22)
{: .text-justify}
- 1 écran LCD 16*2 avec backpack I2C à base de PCF8574
{: .text-justify}
- 2 condensateurs céramiques 100nf
{: .text-justify}
- 4 résistances 10 k ohms
{: .text-justify}
- 2 boutons poussoirs 6mm 4 pattes
{: .text-justify}
- 1 jack Barrel DC
{: .text-justify}
- 1 alimentation 5v 2A jack DC (n’apparaît pas sur la photo)
{: .text-justify}
- 1 barrette femelle 2*20pin pas 2,54mm pour Raspberry pi
{: .text-justify}
- 1 connecteur 4 pin header coudé mâle 2,54mm (pour brancher le LCD)
{: .text-justify}
- 1 connecteur 3 pin header coudé mâle 2,54mm (popur brancher le capteur DHT22)
{: .text-justify}
- des câbles dupond souples femelle/femelle pour raccorder le LCD et le DHT22 à la carte
{: .text-justify}
- un jeux d'entretoises nylons M2.5 pour fixer le LCD (et surélever le PCB si vous n'imprimez pas le boîtier).
{: .text-justify}
- 1 circuit imprimé à faire fabriquer chez un fabriquant de PCB de votre choix, à partir des fichiers GERBER fournis dans le [Github du projet](https://github.com/papsdroidfr/pimometre). Il s'agit du fichier zippé GERBER_pimometre.zip
{: .text-justify}

## Circuit électronique
![Pimometre](/assets/images/tutos/021Pimometre/Kicad_schema.png)
Le LCD (avec son backpack I2C) ainsi que le capteur DHT22 sont connectés à la carte avec des connecteurs souples femelles raccordés sur les connecteurs coudés. La soudure des composants sur la carte est simple car toutes les explications sont sérigraphiées directement sur la carte, il n'y a aucun piège avec les sens à respecter. En dessous de la carte il faut souder le connecteur Jack DC et la barrette 2*20 pin, et tout le reste est au dessus.
{: .text-justify}
{% include gallery id="gallery1" caption="soudure des composants" %}

![Pimometre](/assets/images/tutos/021Pimometre/DHT22.png){: .align-left}
Vous noterez que le capteur **DHT22** est composé de 4 pattes, mais seules 3 sont raccordées à la carte. C'est bien normal, en lisant le datasheet du DHT22 la 3ème patte "NULL" ne doit pas être reliée (ne me demandez pas pourquoi ils en ont mis 4 ...). Il faut donc relier la 1 à VCC, la 2 à SIG, et la 4 à la masse GND sur la carte.
{: .text-justify}

![Pimometre](/assets/images/tutos/021Pimometre/assemblage2.jpg){: .align-right}
Pour assembler le tout, utilisez des petites entretoises en nylon M2.5 15mm pour surélever le LCD. Si vous n'imprimez pas le boitier 3D, pour obtenir un **effet d'inclinaison** il suffit d'utiliser au dessous de la carte deux paires d'entretoises qui ne sont pas de la même taille: le support est ainsi posé incliné et c'est plus agréable pour les yeux. Le Raspberry pi vient s'enficher par dessous sur le connecteur 2*20 pin, **il ne doit pas dépasser de la carte** (sinon c'est qu'il est dans le mauvais sens...)
{: .text-justify}

## Configuration du système
Pour voir tout le détail du projet (circuit électronique, explications pas à pas, tests prototypes et explications sur les scripts Python): je vous encourage à lire toutes les sections du [Github du projet](https://github.com/papsdroidfr/pimometre).
{: .text-justify}
### Étapes de configurations
- Il faut dans un premier temps configurer le piZERO: [cet article](https://papsdroidfr.github.io/configuration/config-pzero/) est dédié à la préparation d'un Raspberry piZERO.
{: .text-justify}
- Pour faire fonctionner l'API de météo, il faut déclarer votre propre TOKEN sur le site [meteo.concept](https://api.meteo-concept.com/). Déclarez un **token standard gratuit** qui vous permet d'interroger l'API jusqu'à 500 fois par jour. Le script Python va interroger l'API toutes les 5mn, ce qui génère au maximum 288 interrogations par jour. Sauvegarder votre token dans un fichier **tokenAPI.txt**. 
{: .text-justify}
- Déposez ensuite dans le répertoire **/home/pi/pimometre** les fichiers **tokenAPI.txt** (avec votre propre token en 1ère ligne),  **I2C_LCD_DRIVER.py**, et **pimometre.py**
{: .text-justify}
- Installez bien toutes les dépendances nécessaires pour faire fonctionner le capteur DHT22, comme expliqué ans la partie "tests" du Github:
{: .text-justify}

Il faut tout d'abord activer l'interface I2C du Raspberry pi avec:
{: .text-justify}
```
sudo raspi-config
```
Reboot nécessaire (sudo reboot)

Il faut installer les dépendances et bibliothèques circuit-python d'Adafruit en suivant [ce guide](https://docs.circuitpython.org/projects/dht/en/latest/). Pour ce qui me concerne j'ai enchaîné ces commandes:
{: .text-justify}
```
sudo pip3 install rpi.GPIO
sudo pip3 install adafruit-blinka
sudo pip3 install adafruit-circuitpython-dht
sudo apt-get install libgpiod2
```

### Démarrage automatique
Le srcipt **pimometre.py** prend en paramètre le **code INSEE** de votre ville (à ne pas confondre avec le CODE POSTAL!). Pour retrouver le code INSEE de votre ville, c'est [par ici](https://public.opendatasoft.com/explore/dataset/correspondance-code-insee-code-postal/table/). Sinon vous pouvez aussi utilisez les scripts de tests fournis dans le Github du projet où l'API est interrogée avec un code postal et fourni en retour les codes INSEE à choisir (un même code postal peut regrouper plusieurs villes).
{: .text-justify}
Pour que le script soit exécuté au démarrage du Raspberry pi, on va passer par la crontab:
{: .text-justify}

Editer la crontab en édition:
```
sudo nano /etc/crontab -e
```

ajouter ces deux lignes à la fin, avant le #:
```
@reboot pi python3 -u 'pimometre/pimometre.py' [INSEE] > 'pimometre/pimometre.log' 2>&1 & 
@daily reboot
```

pensez à remplacer [INSEE] par le code INSEE de votre ville (à ne pas confondre avec le code postal ...). CTRL-O pour sauvegarder les changements, puis CTRL-X pour quitter.
{: .text-justify}

Et voilà, vous pouvez rebooter le Raspberry (sudo reboot) et lorsqu'il démarre la météo doit s'afficher . S'il ne se passe rien au bout de quelques minutes, vous pouvez consulter le fichier de logs /home/pi/pimometre/pimometre.log .
{: .text-justify}

![Pimometre](/assets/images/tutos/021Pimometre/ecran_start.png){: .align-left}
Normalement lorsque le Raspberry démarre le LCD s'allume et vous devez voir en surbrillance la 1ère ligne (pensez à régler la luminosité du LCD sur la petite vis au niveau du backpack I2C).
{: .text-justify}

![Pimometre](/assets/images/tutos/021Pimometre/ecran_Initialisation.png){: .align-left}
Quand le script s'exécute, vous devez voir cet écran: c'est bon signe. La première ligne indique que le DHT22 est en cours d'initialisation (ça peut lui prendre plusieurs secondes avant de fournir un résultat: c'est normal). La seconde ligne indique tout simplement que l'interrogation API n'est pas possible sans le WIFI: c'est normal car les services WIFI prennent du temps à démarrer sur le Raspberry pi, il faut patienter quelques dizaines de secondes tout au plus.
{: .text-justify}

Si au bout de quelques minutes il ne se passe rien, c'est qu'il y a un problème avec l'appel d'API: connectez vous en SSH au piZERO et regardez le fichier de log pimometre/pimometre.log il y a sûrement un message d'erreur. Avez vous bien mis un code INSEE valable? Avez vous bien mis votre TOKEN dans le fichier tokenAPI.txt ?
{: .text-justify}

### Utilisation
- la première ligne indique la température et humidité ambiante captée par le DHT22. S'il apparaît une étoile après le "In" c'est que la dernière mesure a échouée: ça arrive souvent les capteurs DHT22 sont capricieux. Les mesures se font toutes les 10 secondes: l'étoile disparaît dès qu'une mesure est ok, mais vous verrez qu'elle réapparaît souvent.
{: .text-justify}
- la seconde ligne indique les prévisions météo locales via l'API: un premier écran avec la température et l'humidité pendant 2 secondes, puis un second écran avec la vitesse du vent et la probabilité de pluie (2s), un troisième écran avec le bulletin météo en scrolling. La légende à gauche de l'écran indique l'heure de la prévision et une petite icône représentative de ce qui est affiché (thermomètre / soleil / pluie / 5 niveaux de pluie)
{: .text-justify}
- Un appui sur le bouton Select va afficher les prévisions météo dans 3h, puis 6h, 9h et retour à l'heure courante
{: .text-justify}
- un appui sur le bouton Off va éteindre le système. Pour le redémarrer: il faut enlever et rebrancher le +5v.
{: .text-justify}

{% include gallery id="gallery2" caption="Enchaînement des 3 écrans" %}

>En cas de coupure WIFI, vous aurez le message "pb cnx API" affiché sur la seconde ligne. Dès que le WIFI est réactivé tout reprend.
{: .text-justify}

## Boîtier imprimé 3D
![Pimometre](/assets/images/tutos/021Pimometre/boitier3D.jpg)
Si vous êtes équipé d'une imprimante 3D, vous pouvez imprimer ce petit boîtier réalisé sur mesure avec des cotes multiple de 0.2mm, de manière à pouvoir imprimer rapidement sans problème en 200 microns. Je conseille de cocher la case "amélioration de la surface d'adhésion" sinon les bords ont tendance à se soulever un peu lors de l'impression, ce qui rend le boîtier légèrement bombé et très difficile à fermer ensuite vu que tout est calculé au 1/5 de mm près.
{: .text-justify}

Les **fichiers STL** sont dans le dossier **/impression3D** du [Github du projet](https://github.com/papsdroidfr/pimometre).
{: .text-justify}
{% include gallery id="gallery3" caption="Boîtier imprimé 3D" %}

Le boîtier peut être posé à plat, ou bien sur des pieds incliné à 20°, ou encore à 90° (l'ouverture de la prise Jack a été conçue pour) et enfin il peut être fixé sur un mur. Il y a 3 élements à imprimer: la coque basse, la coque haute et les pieds optionnels (à imprimer 2 fois) si vous optez pour une pose inclinée à 20°.
{: .text-justify}

![Pimometre](/assets/images/tutos/021Pimometre/assemblage3D1.jpg){: .align-left}
Une fois la coque basse imprimée (comptez 1h40 en 0.2mm) vous pouvez poser dedans tout le système assemblé avec 4 entretoises M2.5 (M2 aussi ça marche) de 15mm qui surélèvent le LCD. Il faut bien vous assurer que la carte est posée à raz-bord comme sur la photo: elle repose sur un tout petit rebord de 1mm. Si elle est dure à rentrer il faut l'incliner et rentrer de force: ça fait "clac" quand elle est en place. Il y a une toute petite ouverture de chaque côté de 0.4mm pour pouvoir la retirer facilement à l'aide d'un tournevis plat. Assurez-vous que la carte est à peut-près au milieu (ouvertures symétriques de chaque côté) ça va optimiser le positionnement des rehausses des boutons. Si les entretoisent (partie filetée) dépassent trop du bord de la carte il faut les couper sinon ça va gêner la fermeture du boîtier.
{: .text-justify}

![Pimometre](/assets/images/tutos/021Pimometre/assemblage3D2.jpg){: .align-left}
La coque haute (1h50 en 0.2mm) se pose sans problème et se maintient grâce aux petits renfort de 1mm de large qui entrent dans le logement de coque basse: idem ça fait un petit "clac" quand tout est en place. Faites juste gaffe à bien positionner les branchement de la sonde de température dans l'ouverture prévue sur le boîtier et ne pas les pincer entre les deux coques...
{: .text-justify}

Quand aux pieds (10mn les 2 pieds en 0.2mm) ils se positionnent dans leur logement: ça passe juste mais ça rentre en insistant. N’hésitez pas à enlever les bavures d'impression au cutter.
{: .text-justify}