---
#source: https://www.papsdroid.fr/post/iaopencvfacedetection
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
  teaser : "/assets/images/tutos/010IAopenCV/IAopencv-300.jpg"

#SEO tags
title       : "IA OPenCV - détection de visage"
image       : "/assets/images/tutos/010IAopenCV/IAopencv-300.jpg"
description : "Détection de visage avec OpenCV sur un Raspberry pi 4"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "IA OpenCV"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "IA" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pi" , "python3", "openCV" ]

---
![IAOpenCV](/assets/images/tutos/010IAopenCV/IAopencv-300.jpg){: .align-left} 
Une bibliothèque open source dédiée à la reconnaissance de formes en Intelligence Artificielle et qui s'intègre "relativement" facilement sur Raspberry pi: [OpenCV](https://opencv.org/    ) (Open Computer Vision). 
<!--more-->
En quelques lignes de codes Python, on parvient facilement détecter toute sorte d'objets dans une image et ça ne pose aucun problème pour nos nano-ordinateurs Raspberry pi qui ne payent pas mine.
{: .text-justify}

## Installer OpenCV sur un Raspberry pi
Il y a deux façons de procéder: une musclée qui prend à peu près 4h en compilant les sources (c'est la méthode décrite dans le site officiel OpenCV): elle permet d'être très précis sur sa configuration. Heureusement il y a une autre méthode beaucoup plus simple qui sera adaptée pour un usage "normal", à partir de complilations déjà prêtes à l'emploi. Mais il y aura toutefois quelques dépendances à installer avant: c'est cette méthode que j'ai adoptée.
{: .text-justify}

Elle est parfaitement décrite dans [cet article](https://pyimagesearch.com/2018/09/19/pip-install-opencv/), dans plusieurs configurations dont les Raspberry pi. A noter que la technique d'installation décrite pour un **Raspberry pi4** ne fonctionne pas (pas chez moi en tous cas ...). J'ai eu beau chercher dans tous les forums, beaucoup rencontrent des pb avec OpenCV sur un Raspberry pi4, et je n'ai pas trouvé de solution qui tienne la route: ça fini toujours pas merder quelque-part alors j'ai jeté l'éponge et me suis concentré sur une installation avec un **Raspberry pi3** équipé d'une SD card de 16Go.
{: .text-justify}

## Configuration
D'abord on prend la bonne habitude de mettre à jour son système, c'est LA bonne habitude à prendre quand on a une série de "sudo apt-get install" .... qui s'enchaînent.
{: .text-justify}
```
sudo apt-get update && sudo apt-get upgrade
```

Ensuite les dépendances à installer pour qu'OpenCV fonctionne:
{: .text-justify}
```
sudo apt-get install libhdf5-dev libhdf5-serial-dev libhdf5-100   
sudo apt-get install libqtgui4 libqtwebkit4 libqt4-test python3-pyqt5
sudo apt-get install libatlas-base-dev
sudo apt-get install libjasper-dev
```

On peut alors installer OpenCV en passant par l'installation "pip" qui va chercher les compilations déjà préparées. Il est de bon ton d'installer ou de mettre à jour pip avec la dernière version:
{: .text-justify}
```
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
```

Et là on lance l'installation d'OpenCV avec pip, qui va prendre quelques secondes au lieux de 4h ...
{: .text-justify}
```    
sudo pip install opencv-contrib-python
```

Vous pouvez rencontrer une erreur qui indique qu'il n'y a pas assez de place sur l'espace disque ... c'est assez classique c'est la mémoire cache limitée par défaut à 10mo qui est insuffisante (j'ai eu ce problème) et il suffit de la paramétrer à 100mo et de relancer le sudo pip install opencv-contrib-python. La mémoire cache temporaire se paramètre dans le fichier fstab. Si vous avez suivi [ces conseils](https://www.magdiblog.fr/divers/comment-prolonger-la-duree-de-vie-de-vos-cartes-sd-sur-raspberry-pi/) indispensables pour protéger sa carte SD (à faire absolument ....) normalement dans votre /etc/fstab vous devez avoir ces lignes: (faite un sudo nano /etc/fstab pour le lire et le modifier)
{: .text-justify}
```
tmpfs /tmp tmpfs defaults,noatime,nosuid,size=10m 0 0
tmpfs /var/tmp tmpfs defaults,noatime,nosuid,size=10m 0 0
tmpfs /var/log tmpfs defaults,noatime,nosuid,mode=0755,size=10m 0 0
```

la 1ère ligne où il y a le point de montage /tmp il suffit de remplacer 10m par 100m et le tour est joué, mais il faut rebooter le pi pour que ce soit pris en compte. Ensuite quand l’installation est terminée pensez à revenir en arrière en remettant la mémoire cache à 10m car les pi3 ne sont pas très fournis en RAM....
{: .text-justify}

Pour tester que tout fonctionne: dans une console entrez les deux commandes suivantes
{: .text-justify}
```python
python3
import cv2
```
> si pas de message d'erreur: tout va bien (sur un pi4 par exemple il y a des messages d'erreurs en pagaille).
{: .text-justify}

## Détection de visage capté par une WebCam USB
Le programme ci-dessous va permettre en quelques lignes de codes d'identifier un visage sur le flux vidéo capté par une webcam (USB dans mon cas). Il utilise pour cela un **Classifieur** déjà entraîné à reconnaître des visages de face, et qu'il convient de charger. Les classifieurs de reconnaissance de forme sont disponibles sur cette [page gitHub](https://github.com/opencv/opencv/tree/master/data/haarcascades). J'ai utilisé le classifieur **haarcascade_frontalface_default.xml** qu'il vous faut télécharger et installer au même endroit que le programme python ci-dessous.
{: .text-justify}

Ensuite téléchargez ce [programme python](https://github.com/papsdroidfr/FaceDetectionOpenCV), branchez une webcam sur un port USB disponible: et tous les visages qui passeront dans le champs de vision de la webcam seront détectés, encadrés en vert. Pour sortir de ce programme: appuyez sur la touche "q" de votre clavier.
{: .text-justify}

{% include video id="tgZN4jEbwiY" provider="youtube" %}