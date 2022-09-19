---
#source: https://www.papsdroid.fr/post/configurer-pizero
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
  teaser : "/assets/images/tutos/020PiZero/pizero-300.jpg"

#SEO tags
title       : "Configurer un Raspberry pi zéro"
image       : "/assets/images/tutos/020PiZero/pizero-300.jpg"
description : "Méthode pas à pas pour configurer un Raspberry pi zéro"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "pzero"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "configuration" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pzero"]
---
![pzero](/assets/images/tutos/020PiZero/pizero-300.jpg){: .align-left} 
Quand on démarre un projet à base d'un **Raspberry pi zero** il y a d'abord toute l'étape de configuration du Raspberry qui demande par mal de manipulations. Afin d'éviter d'avoir à le répéter dans tous mes tutos, je détaille ici ma méthode pas à pas.
{: .text-justify}

Il va falloir dans un premier temps acquérir du matériel pour faciliter la configuration, puis installer le système d'exploitation, configurer un accès SSH sécurisé afin d'accéder au piZero depuis un autre ordinateur, protéger la fragile carte SD, et enfin configurer les outils de développement python lorsque le projet implique d'exécuter des scripts python.
{: .text-justify}

## Choix du Raspberry piZERO
Attention il y a plusieurs versions de Raspberry piZERO, ne vous faite pas piéger surtout si vous les achetez loin sur des sites (très) mal traduits et peu documentés (si vous voyez un vendeur vendre des "framboises": fuyez ...). 
{: .text-justify}

Si votre projet nécessite un accès internet, soyez sûr d'acheter une version Raspberry pi ZERO **W**: W signifie **WIFI**. Sinon vous l'aurez dans l'os question accès internet, à moins de rajouter un dongle WIFI en USB ce qui n'a aucun intérêt car ça va doubler le prix d'une version W de base.
{: .text-justify}

Si vous avez besoin d'accéder au port GPIO, prenez alors une version pi ZERO **WH**: le H signifie qu'une barrette pin **Header** 2*20 broches est déjà soudée. Si vous êtes équipé, vous pouvez toujours souder vous même une barrette pin header 2*20 broches vous économiserez 5€ sur le prix: ce n'est pas si compliqué à réaliser avec un bon fer à souder.
{: .text-justify}

## Installer l'OS sur une carte SD
Dans un premier temps il faut installer l'os **Raspberry pi OS Lite** sur la carte SD (16 Go suffisent): pas besoin d'interface graphique avec un piZERO. Tout est bien expliqué sur le site [Raspbian](https://www.raspberrypi.com/software/). J'encourage fortement à utiliser l'utilitaire Pi Imager (multi-platefrome, même sur un un autre Raspberry pi ça fonctionne). Lorsque la carte SD est prête, vous avez deux options selon votre "apétence" à modifier les fichiers de configuration ou non: explications ci-dessous
{: .text-justify}

### Option n°1: utilisation d'un écran/clavier/souris et Ethernet
![pzero](/assets/images/tutos/020PiZero/pZeroClavier.jpg){: .align-left} 
Cette option permet une installation directe sans aucune manipulation de fichiers de configuration préalable, mais il faut acquérir du matériel qui s'avère plus cher que le pZERO lui-même (une bonne trentaine d'€). Moi par exemple j'en configure à la pelle des pZERO donc ce matériel est bien amorti.
{: .text-justify}

Les deux éléments absolument indispensables à acheter dans ce cas c'est un HUB usb avec prise ethernet rj45 intégré, ainsi qu'un adaptateur mini-HDMI vers HDMI.
{: .text-justify}
- **Hub USB avec prise ethernet**: le piZEO est équipé d'un seul port micro-USB OTG (à côté de l'alimentation au format micro-usb aussi). Cet objet fantastique va vous permettre de transformer cet unique micro-USB OTG en 3 prises USB + une prise réseau ethernet: branchez y un clavier USB, une souris USB et une prise réseau ethernet, et vous aurez votre souris, clavier et accès internet directement opérationnels sans aucune manip: très pratique !
{: .text-justify}
- **mini-HDMI vers HDMI**: la gestion des HDMI chez Raspberry c'est un peu le bordel ... on a du mini-HDMI avec un piZERO, du HDMI avec les pi3 et du double micro-HDMI avec les pi4 ! Bien sûr tous nos moniteurs et PC sont eux en HDMI. Ce convertisseur évite d'acheter un câble mini-HDMI vers HDMI: on peut utiliser un câble HDMI qu'on a déjà.
{: .text-justify}
- **Alimentation**: une alim 5v pour Raspberry pi3 fera parfaitement l'affaire.
{: .text-justify}

Insérez la carte SD dans le pZERO, Branchez clavier, souris, prise ethernet, branchez un écran , et en dernier l'alimentation 5v: le pZERO démarre et vous voyez ce qu'il se passe à l'écran.
{: .text-justify}

Le user par défaut est pi, et le password par défaut est raspberry. Si le password raspberry ne passe pas essayez rqsberry: ça arrive une fois sur deux que le clavier soit configuré en qwerty au lieu d'azerty au démarrage.
{: .text-justify}

### Option n°2: utilisation d'un simple câble micro-USB/USB
![pzero](/assets/images/tutos/020PiZero/pZeroCable.jpg){: .align-left} 
Si ça vous embête d'investir dans 30€ de matériel pour configurer un pZERO qui en vaut 10 ou 15€ ... sachez qu'il est possible de tout faire avec **un seul câble micro-usb vers USB** que l'on va brancher sur la prise micro-USB OTG (et non pas sur la prise PWR!) et reliée à un ordinateur. Mais avant d'insérer la carte SD dans le PZERO il est nécessaire de modifier quelques fichiers de configuration (étape inutile avec l'option 1): à vous de choisir ....
{: .text-justify}

Lorsque l'image de l'OS est créé sur la carte SD par l'utilitaire pi Imager, vous devez voir deux partitions boot et rootfs. Allez dans la partition boot, puis 
{: .text-justify}

Ouvrez avec un éditeur texte le fichier **config.txt**, ajoutez y cette ligne à la fin:
{: .text-justify}
```
 dtoverlay=dwc2
```

Ouvrez ensuite le fichier **cmdline.txt**, il y a tout un tas de commandes qui sont à la queue-leue-leue sur une seule ligne. Allez à la fin, ajoutez (avec un espace avant pour séparer du reste de la ligne): 
{: .text-justify}
```
modules-load=dwc2,g_ether 
```

créez un fichier vide sans aucune extension nommé **ssh**
{: .text-justify}

Insérer ensuite la carte SD dans le pZERO, branchez le câble micro-usb (prise usb du PZERO et rien sur la prise PWR), branchez le câble USB à votre ordinateur: le pZERO démarre ! Attendez que la lumière verte ne clignote plus et vous pouvez vous connecter en SSH depuis votre ordinateur en entrant cette commande: 
{: .text-justify}
```
ssh pi@raspberrypi.local
```

Le password est raspberry (essayez rqsberry si ça ne passe pas ....)
{: .text-justify}

Vous êtes ainsi connecté au piZERO en utilisant le clavier/écran/souris/ethernet de votre propre PC.
{: .text-justify}

## Configuration du système
### initialisation
Que vous soyez connecté via SSH (option 2) ou en direct avec un écran/clavier/souris vous pouvez enchaîner les étapes de configurations suivantes:
{: .text-justify}
- Exécutez **sudo raspi-config** pour modifier la langue, le clavier (AZERTY) et le password du user (mettez un password robuste), modifier le nom du piZERO qui apparaîtra sur le réseau (un nom en rapport avec votre projet), et **activer SSH** ainsi que le **WI-FI**. La connexion SSH va permettre de se connecter sur le piZERO à distance sans avoir à rebrancher souris/clavier/écran.
{: .text-justify}
- Réflexe de sécurité: **modifier le port SSH par défaut (22)** par une valeur supérieur à 1024. Ça évite les intrus qui viennent tester le port 22. C'est bien expliqué sur le [site de Framboise314](https://www.framboise314.fr/securiser-son-raspberry/). 
{: .text-justify}
- Faire une mise à jour (c'est long!) du Raspberry pi: **sudo apt-get update** suivi d'un **sudo apt-get upgrade**.
{: .text-justify}
- Enfin pour finir il faut sécuriser sa carte SD en déportant les écritures de logs dans la RAM en suivant [ce guide](http://www.magdiblog.fr/divers/comment-prolonger-la-duree-de-vie-de-vos-cartes-sd-sur-raspberry-pi/) très simple. Pour éditer le fichier /etc/fstab entrez la commande: **sudo nano /etc/fstab** et ajoutez y les 3 lignes indiquées, puis CTRL-O pour sauver et CTRL-X pour quitter.
{: .text-justify}

Vous pouvez rebooter votre piZERO (commande: **sudo reboot**) ou bien l'éteindre (commande: **sudo poweroff**). 
{: .text-justify}
>Ne l'éteignez jamais "sauvagement" en débranchant le câble d'alimentation: il y a un gros risque de corrompre la carte SD et il faudrait tout réinstaller depuis le début.

### Configuration accès distant SSH
Lorsque tout est configuré, débranchez clavier/écran/souris ainsi que la prise ethernet dans le cas de l'option 1, puis allumez votre piZERO, attendre que la lumière verte ne clignote plus du tout: le pi est démarré. Il est conseillé d'accéder à l'interface de votre BOX internet afin **d'allouer une IP fixe** (voir les paramètres **DHCP** de votre BOX), car il faut connaître l'IP de son piZERO pour pouvoir s'y connecter en SSH. Si vous avez donné un nom à votre piZERO, vous devriez pouvoir le repérer facilement via votre box internet et identifier son adresse IP qui devrait ressembler à quelque-chose comme **192.168.1.xx** sur votre réseau local. Si vous n'allouez pas une IP fixe, le xx peut changer après un reboot.
{: .text-justify}

>Il est préférable que ce soit votre BOX qui alloue une IP fixe plutôt que de bidouiller les paramètres IP au niveau du piZERO pour forcer une IP, car vous risquez de faire des bêtises avec une IP déjà allouée par ailleurs.
{: .text-justify}

Si vous n'arrivez pas à identifier l'IP du piZERO au niveau de la BOX internet, c'est peut être qu'il y a un pb de connexion WIFI... rebranchez le HUB USB avec prise ethernet pour en avoir le cœur net: s'il apparaît c'est que vous devez reconfigurer le WIFI du piZERO en rebranchant écran/clavier/souris.
{: .text-justify}

Une autre méthode pour s'assurer que le piZERO est bien connecté au WIFI (débranchez bien la prise ethernet du HUB USB) est d'exécuter la commande:
{: .text-justify}
```
hostname -I
```
si vous voyez une réponse qui commence par 192.168.1.xx c'est que votre accès WIFI fonctionne et vous avez votre adresse IP au passage: vous pouvez retirer écran/clavier/souris et vous connecter au piZERO depuis un autre ordinateur via un utilitaire SSH.
{: .text-justify}

Si vous utilisez un ordinateur linux ou un Raspberry pi4 (c'est mon cas), pour se connecter en SSH au piZERO rien de plus simple, il suffit d'ouvrir un terminal et d'entrer cette ligne de commande:
{: .text-justify}
```
ssh -pXXXXX pi@192.168.1.yy
```
où XXXXX correspond au port SSH que vous avez modifié (22 par défaut) et 192.168.1.yy correspond à l'adresse IP du piZERO. Répondez "Yes" s'il vous pose des questions.
{: .text-justify}

>Vous êtes alors connecté à votre piZERO depuis ce terminal: ça signifie que toutes les commandes que vous entrez dans ce terminal vont s'exécuter sur le piZERO.
{: .text-justify}

Vous pouvez aussi être amené à déposer des fichiers sur votre piZERO (exple après clonage d'un projet Github): dans ce cas l'utilisation d'un client FileZilla avec connexion sftp qui utilise la connexion SSH du piZERO est très utile: vous pouvez voir l’arborescence des fichiers sur le piZERO et agir à distance.
{: .text-justify}

Pour éteindre proprement le piZERO: 
{: .text-justify}
```
sudo poweroff
```

et vous pouvez débrancher l'alimentation lorsque la petite lumière verte est éteinte (elle clignote pendant la phase d'extinction).
{: .text-justify}

## Pour les développeurs PYTHON
### Editeur nano
Si comme moi vous aimez programmer l'accès au GPIO en Python, il est utile de configurer l'éditeur **nano** auquel vous accéderez en SSH depuis un autre ordinateur. Une fois connecté en SSH au piZERO, nous allons configurer l'éditeur nano pour qu'il numérote les lignes, utilise un jeu de couleur approprié, indente en utilisant 4 espaces, et reconnaisse l'utilisation de la souris (très pratique) à l'intérieur de l'éditeur.
{: .text-justify}

Si vous ne connaissez pas nano: [ce tuto](https://www.hostinger.fr/tutoriels/nano) s'impose.
{: .text-justify}

Pour configurer nano, on passe par la configuration globale dans /etc/nanorc
{: .text-justify}
```
sudo nano /etc/nanorc
```

suivez le tuto indiqué plus haut pour activer toutes les options intéressantes (auto-indentation, souris activée, numérotation des lignes, indentation "python" avec 4 espaces, coloriage syntaxique ...)
{: .text-justify}

>Votre piZERO est prêt vous pouvez y coder votre projet à distance via SSH et nano ou bien y déposer des scripts pythons via un utilitaire type FileZila .
{: .text-justify}

### Exécuter une commande automatique au démarrage
Pour qu'une commande soit exécutée au démarrage du pi ou à intervalle régulier, il faut ajouter des commandes dans la CRONTAB.
{: .text-justify}

Pour entrer dans la crontab en édition depuis le piZERO, entrez la commande:
{: .text-justify}
```
sudo nano /etc/crontab -e
```
vous pouvez alors y entrer vos commandes.
{: .text-justify}

le raccourci **@reboot** permet d'exécuter une commande au reboot du pi.
{: .text-justify}

Par exemple **@reboot pi python3 monscript.py &** va provoquer l'exécution du programme python monscript.py (en supposant qu'il ait été déposé dans /home/pi), par le user pi, au démarrage du piZERO ! Ne pas oublier le & à la fin de la commande: il signifie d'exécuter la commande en tâche de fond sinon le piZERO reste bloqué sur l'exécution de cette commande.
{: .text-justify}

Si ça ne se passe pas comme prévu, il y a sûrement un plantage qu'il faut analyser: dans ce cas il faut rediriger les logs dans un fichier pour pouvoir le consulter, la commande se complique un tout petit peu:
{: .text-justify}
```
@reboot pi python3 -u monscript.py > monscript.log 2>&1 &
```
Cette commande redirige toutes les sorties (standard + les erreurs) dans le fichier monscript.log qui va être créé (en annule et remplace s'il existe déjà) dans /home/pi. Les causes d'erreurs peuvent être nombreuses: peut-être qu'il faut les droits root pour exécuter le script et dans ce cas remplacer pi par root, et mettre le chemin complet où se situe le script. Il y a aussi peut-être de dépendances avec des démarrages de services à gérer (exemple typique: le WIFI!) et s'ils ne sont pas encore démarrés quand le script est exécuté: ça plante.
{: .text-justify}

Il suffit alors d'aller consulter ce qui se passe dans le fichier **/home/pi/monscript.log** avec la commande **sudo nano /home/pi/monscript.log**
{: .text-justify}

Les raccourcis **@weekly**, **@monthly** etc.. permettent d'exécuter une tâche hebdo ou mensuelle. Si votre piZERO tourne h24 7J/7 avec des log qui se replissent (exple d'un serveur), il est fortement conseillé de programmer un reboot de temps en temps. **@montly reboot** dans la crontab va provoque le reboot automatique du piZERO tous les mois: super pratique la crontab !
{: .text-justify}

