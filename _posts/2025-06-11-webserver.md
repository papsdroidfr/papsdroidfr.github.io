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
  teaser : "/assets/images/tutos/042WebServer/python_flask_300.jpg"

#SEO tags
title       : "Web Server Python et Flask"
image       : "/assets/images/tutos/042WebServer/python_flask_300.jpg"
description : "Structure d'un wweb server écrit en Python et Flask"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Web Server"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "DEV" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-python" "électronique"
tags        : ['python3', 'raspberry pi',  'web server']

gallery_bashtop:
  - url: /assets/images/tutos/042WebServer/bashtop_1024.png
    image_path: "/assets/images/tutos/042WebServer/bashtop_640.png"
    title: "bashtop"

---

![webserver](/assets/images/tutos/042WebServer/python_flask_300.jpg){: .align-left}
Cet article détaille comment monter un **server web local** sur un raspberry pi en moins de **5mn**, avec Python et Flask. Ce serveur ne sera pas ouvert à l'extérieur, donc on va aller au plus simple niveau architecture: pas de reverse proxy, pas de serveur de réplication WYSIWIG et pas de serveur de base de données: tout va tenir sur un seul Raspberry pi. L'objectif est de pouvoir consulter depuis son smartphone ou un ordinateur connecté au même réseau local les services de ce serveur web.
{: .text-justify}

## Hardware

Un simple raspberry pi avec une connectivité wifi ou réseau est nécessaire. Dans mon cas je suis parti avec un [Raspberry pi4 8Go dans un boîtier Argon One](https://papsdroidfr.github.io/configuration/argon-one/) où le système est installé sur un SSD M.2 nvme de 128Go. Cette configuration me permet de bénéficier d'une solution fanless. Une sd card de 32Go est largement suffisante mais l'installation et le démarrage du système est beaucoup plus rapide avec un ssd M.2 nvme. J'ai utilisé l'OS LITE 64bit Raspberry pi OS car je n'ai pas besoin d'interface graphique.
{: .text-justify}

## Software

### Structure
![webserver](/assets/images/tutos/042WebServer/structure_640.png){: .align-center}

La structure du code minimale pour notre server web est la suivante:
{: .text-justify}

* Le programme principal **demo.py** à la racine.
{: .text-justify}
* Les dépendances à installer si vous passez par un [environnement virtuel](https://docs.python.org/fr/3.9/library/venv.html): **requirements.txt**.
{: .text-justify}
* Un dossier **/static** dans lequel figure le dossier **/css** avec les styles de nos pages.
{: .text-justify}
* Un dossier /templates dans lequel figurent les pages html du site.
{: .text-justify}

### Installation

![webserver](/assets/images/logos/github-flat-shadow-octocat-emblem-64.png){: .align-left}
Tout le code démo est à récupérer sur mon [Github](https://github.com/papsdroidfr/rascrypto), dans le dossier **/demo**
{: .text-justify}

Déposez tous les fichiers du répertoire **/demo** en respectant bien la structure décrite ci-dessus.

Si vous n'utilisez pas d'environnement virtuel (venv), il faut installer la bibliothèque FLask.
{: .text-justify}

```bash
pip install Flask
```

Sinon, il suffit d'installer les dépendances du requirements.txt:
{: .text-justify}
```bash
pip install -r requirements.txt
```

### Usage

pour démarrer le serveur WEB, rien de plus simple, il suffit d'exécuter le programme **demo.py** 
{: .text-justify}

```python
python demo.py
```

Le site WEB est alors accessible en local sous l'adresse **http://127.0.0.1:5000**
{: .text-justify}

![webserver](/assets/images/tutos/042WebServer/demo_640.png){: .align-center}

Un bashtop montre que cette configuration n'occupe que 1Go de RAM et 12% de CPU (sachant que bashtop perturbe bien la mesure)
{: .text-justify}
{% include gallery id="gallery_bashtop" caption="Cliquez pour agrandir l'image." %}