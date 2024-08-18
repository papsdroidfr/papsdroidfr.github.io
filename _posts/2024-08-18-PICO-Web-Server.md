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
  teaser : "/assets/images/tutos/037ESP32Veilleuse/veilleuse_300.jpg"

#SEO tags
title       : "PICO Web Server"
image       : "/assets/images/tutos/036ESP32meteo/boitier3D_300.jpg"
description : "Un serveur WEB géré par un Raspberry PICO"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "PICO Web Server"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "DEV" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-pyhton" "électronique"
tags        : ["micro-python", "raspberry pico"]

---

![PICOWeb](/assets/images/tutos/039PicoWeb/screenshot1_300.png){: .align-left}
Ce module écrit en **MicroPython** permet de gérer un **serveur WEB** sur un **Raspberry PICO** afin de prendre le contrôle des commandes depuis son smartphone, par exemple pour commander un moteur. Ce serveur va écouter sur le thread principal les requêtes reçues d'une connexion externe http et générer de manière dynamique une page HTML et un évènement en réponse à la requête http reçue. Les événements générés par le serveur WEB sont lus en boucle dans un second Thread (**multithread**) qui va agir en fonction de l'événement lu: comme par exemple démarrer un moteur, ou l'arrêter etc...
{: .text-justify}

## Hardware

Un **Raspberry PICO** version 1 W(H) ou encore version 2, ainsi qu'une connexion WIFI. L'avantage d'utiliser un Raspberry PICO est qu'il dispose d'un double cœur: le multithread y est particulièrement efficace. Si vous utilisez un ESP32-S3 (aussi double cœur), il faut juste adapter le code de la led.py qui est spécifique à la led du PICO. Si vous utilisez un ESP32-C3 mono-cœur: les Threads vont se marcher sur les pieds et rendre l'expérience assez désastreuse (serveur WEB qui ne répond pas tout de suite, ou événement traité par saccades et ralentissements)
{: .text-justify}

> A l'heure où j'écris cet article je n'ai pas encore reçu de **PICO version 2** (pré-commandes fin août 2024). Une version PICO 2 est vivement conseillée car il dispose d'une **architecture de sécurité** basée sur Arm TrustZOne qui permet d'isoler et sécuriser tout le code micropython, et surtout le mot de passe de connexion wifi, ce que ne propose ni la version 1 du PICO, ni les ESP32-S3 où les password wifi circulent soient en clair dans les fichiers de configuration, ou alors cryptés mais avec le code python de décryptage ainsi que les clés à disposition: moi ça me prendrait 5 secondes à craquer un tel code crypté de cette façon.
{: .text-justify}

## Software

Installez bien la dernière version MicroPython sur votre Raspberry PICO. Cet [article](https://papsdroidfr.github.io/configuration/pico/) détaille comment s'y prendre.
{: .text-justify}

Tout le code micropython est à récupérer sur le [Github du projet](https://github.com/papsdroidfr/PICO-Webserver).
{: .text-justify}

Il faut ensuite importer tout le module web à la racine de son PICO: créer le dossier /web et y importer tous les programmes du module.
{: .text-justify}

- **\_\_init\_\_.py**: initialisation du module
  {: .text-justify}
- **config.py**: variables globales utilisées par le module, on y retrouve le SSID et le PASSWORD wifi à utiliser, et qui serait en bien meilleure sécurité sur un Raspberry PICO 2.
  {: .text-justify}
- **log.py**: gestion interne des messages.
{: .text-justify}
- **led.py**: gestion de la led du PICO (à adapter si vous n'utilisez pas un PICO)
{: .text-justify}
- **server.py**: gestion du serveur web avec une page HTML par défaut "hello PICO WWWorld!"
{: .text-justify}
- **wifi.py**: gestion du WIFI.

### Usage

Le principe du serveur et géré dans le programme **server.py** où deux classes principales sont définies:
{: .text-justify}

- **classe Event()**: Gère la création des évènements par le serveur. Cette classe défini le dictionnaire des actions à exécuter pour chaque évènement.
{: .text-justify}
- **classe Server()**: Gère tout le serveur Web en commençant par se connecter au WIFI, puis démarre dans un Thread dédié la lecture en boucle des événements pour agir en conséquence, et démarre ensuite le serveur WEB qui écoute les requêtes reçues dans le Thread principal, pour générer un code HTML dynamique ainsi qu'un événement adéquat.
{: .text-justify}

> Un exemple très simple vous permet de démarrer un serveur WEB par défaut:
{: .text-justify}
```python
from web.wifi import Wifi
from web.server import Server

my_server = Server(wifi=Wifi(), debug=True)
```
![PICOWeb](/assets/images/tutos/039PicoWeb/screenshot1_640.png){: .align-center}
![PICOWeb](/assets/images/tutos/039PicoWeb/log1.png){: .align-center}


> On ne retrouve pas toutes les fonctionnalités de Python dans MicroPython, il n'y a pas de notion de **classes abstraites** qui eut été bien utile dans mon cas pour définir les classes génériques qui gèrent le serveur WEB et les évènements. Pour contourner cette limitation il suffit de recréer vos propres classes  qui héritent chacune des classes Server() et Event() et de surcharger les fonctions principales, notamment le **dictionnaire des évènements** de la classe Event() et les méthodes **Server.\_thread\_loop\_event(self)** (réaction aux événements) ainsi que **Server.\_gen\_html()** (le générateur dynamique de code HTML)
{: .text-justify}

### Exemple de code

Dans l'exemple de code ci-dessous, je souhaite créer un serveur WEB avec **5 événements** pour contrôler le moteur d'un planétaire mécanique Soleil-Terre-Lune afin de simuler une journée, une semaine, un mois, ou bien de mettre en marche le moteur et bien entendu de pouvoir l'arrêter à tout moment.
{: .text-justify}

![PICOWeb](/assets/images/tutos/039PicoWeb/screenshot2.png){: .align-center}

```python
import time
from web.wifi import Wifi
from web.server import Event, Server


class EventPlanetarium(Event):
    """ This class manages event raised by the webserver to run planetarium"""
    def __init__(self)-> None:
        #Events managed by the web server
        super().__init__()
        
        #my own events
        self.EVENT_RUN1D: str = 'Tourne 1 jour'    #run planetarium for 1 day
        self.EVENT_RUN1W: str = 'Tourne 1 semaine' #run planetarium for 1 week
        self.EVENT_RUN1M: str = 'Tourne 1 mois'    #run planetarium for 1 month
        self.EVENT_RUN:   str = 'En marche'        #run planetarium
        self.EVENT_STOP:  str = 'STOP'             #stop planetarium
        
        #dictionnary of actions for each event name
        self.actions={
            self.EVENT_NONE: self.action_none,
            self.EVENT_RUN1D: self.action_run_1d,
            self.EVENT_RUN1W: self.action_run_1w,
            self.EVENT_RUN1M: self.action_run_1m,
            self.EVENT_RUN: self.action_run,
            self.EVENT_STOP: self.action_stop,
        }

    def action_run_1d(self):
        print('Start run 1D')
        time.sleep(0.5)
        print('End run 1D')
    
    def action_run_1w(self):
        print('Start run 1W')
        time.sleep(1)
        print('End run 1W')

    def action_run_1m(self):
        print('Start run 1M')
        time.sleep(4)
        print('End run 1M')

    def action_run(self):
        print('Start run')
        while self.event_raised == self.EVENT_RUN:
            print('...press STOP...')
            time.sleep(0.5)

    def action_stop(self):
        print('Action STOP')
    

class ServerWeb(Server):
    
    def __init__(self, debug:bool = False)-> None:
        super().__init__(wifi=Wifi(), event = EventPlanetarium(), debug=debug)
     
    def _gen_html(self)-> str:
 
        html = f"""<html>
            {self.HEAD_HTML}
            <body>
                <h1>Soleil-Terre-Lune</h1>
                <p><a href=https://papsdroidfr.github.io/tutoriels/soleil-terre-lune/> <Construire la maquette> </a></p>
                <p>Action en cours: <strong> {self.event_raised} </strong></p>
                <p><a href="/?event=runFull"><button class="button">Marche</button></a></p>
                <p><a href="/?event=run1D"><button class="button">1 jour</button></a></p>
                <p><a href="/?event=run1W"><button class="button">1 semaine</button></a></p>
                <p><a href="/?event=run1M"><button class="button">1 mois</button></a></p>
                <p><a href="/?event=stop"><button class="button button_red">STOP</button></a></p>
            </body>
        </html>"""
        
        return str(html)

    def _raise_event(self, request:str)-> None:
        """ Set event raised, depends on request"""
        if request.find('GET /?event=runFull') > -1:
            self.event.set_event(self.event.EVENT_RUN)
        elif request.find('GET /?event=stop') > -1:
            self.event.set_event(self.event.EVENT_STOP)
        
        elif self.event_raised != self.event.EVENT_RUN:
            if request.find('GET /?event=run1D') > -1:
                self.event.set_event(self.event.EVENT_RUN1D)
            elif request.find('GET /?event=run1W') > -1:
                self.event.set_event(self.event.EVENT_RUN1W)
            elif request.find('GET /?event=run1M') > -1:
                self.event.set_event(self.event.EVENT_RUN1M)
            else:
                self.event.set_event(self.event.EVENT_NONE)
        
        time.sleep(0.2)
    
            
#start web serveur
my_server = ServerWeb(debug=True)
```

Dans cet exemple, les actions ne sont matérialisées que par des "print" dans la console et un temps d'attente. Pour ce qui est de la réaction aux évènements, je met la priorité sur les événements STOP et MARCHE. Tous les autres évènements sont ignorés dès que l'utilisateur à cliqué sur le bouton "MARCHE": il faudra d'abord appuyer sur STOP.
{: .text-justify}

### Pour aller plus loin

Dans un projet finalisé il n'y aura pas besoin de faire tous ces print dans la log: il faudra instancier le ServerWeb avec l'option debug=False. Pour récupérer l'adresse du serveur WEB généré par le PICO il y a deux façon de faire:

- Utiliser un petit écran OLED pour y afficher l'adresse du serveur WEB.
{: .text-justify}
- Si vous utilisez un smartphone Android, l'application "**Net Analyser**" analyse et scanne tous les serveurs LAN actifs sur le réseau WIFI: on détecte ainsi facilement l'adresse du serveur WEB géré par le PICO.
{: .text-justify}