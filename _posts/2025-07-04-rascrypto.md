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
  teaser : "/assets/images/tutos/043Rascrypto/rascrpyto_300.png"

#SEO tags
title       : "Cryptomonnaie - Analyse Technique - Raspberry pi"
image       : "/assets/images/tutos/043Rascrypto/rascrpyto_300.png"
description : "Web Server d'analyse technique de cours de cryptomonnaie"

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Rascrypto"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "DEV" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-python" "électronique"
tags        : ['python3', 'raspberry pi',  'web server', 'cryptomonnaie', 'Flask']

gallery_graphique:
  - url: /assets/images/tutos/043Rascrypto/rascrpyto_1024.png
    image_path: "/assets/images/tutos/043Rascrypto/rascrpyto_800.png"
    title: "bashtop"
---

![rascrypto](/assets/images/tutos/043Rascrypto/rascrpyto_300.png){: .align-left}
Cet article détaille la procédure complète pour installer, configurer et exécuter l'application **RasCrypto**, un serveur local d'analyse technique sur la cryptomonnaie, hébergé sur un Raspberry pi4 avec au minimum 4Go de RAM. Ce projet fonctionnera parfaitement avec un Raspberry pi5, mais sera trop limité sur un pi3 car il faut manipuler d'importantes quantité de données.
{: .text-justify}

## Avertissement

> Ce projet et ce guide sont fournis à des fins purement éducatives. Ils ont pour but de montrer comment collecter des données de marché sur les crypto actifs, effectuer des calculs d'analyse technique et automatiser des tâches comme l'envoi de reports par e-mail avec Python sur un Raspberry Pi. Les informations et analyses générées par cette application ne constituent en aucun cas un conseil en investissement. L'investissement dans les cryptomonnaies est **très risqué** et vous devez faire vos propres recherches. L'auteur de ce projet et de ce guide ne saurait être tenu responsable de toute décision financière que vous prendriez.
{: .text-justify}

## Résumé Fonctionnel et Technique

### Résumé Fonctionnel

**RasCrypto** est une application web locale conçue pour automatiser l'analyse technique de paires de cryptomonnaies (exple: BTCUSDC, ETHEUR ...). Ses fonctionnalités principales sont :
{: .text-justify}

* **Serveur web local**: l'application se consulte via un navigateur web connecté au même réseau local: elle n'est pas ouverte sur l’extérieur.

* **Génération de graphiques**: Elle crée des graphiques dynamiques en chandeliers (OHLCV - Open, High, Low, Close, Volume) pour visualiser l'évolution du cours d'une cryptomonnaie sur une période donnée (jour, semaine, mois, année(s)).
{: .text-justify}

* **Calcul d'indicateurs techniques**: Sur ces graphiques, elle superpose plusieurs indicateurs clés de l'analyse technique :
{: .text-justify}
> **Moyennes Mobiles** (court terme et long terme)
{: .text-justify}

> **Niveaux de Support et de Résistance**
{: .text-justify}

> **RSI** (Relative Strength Index)
{: .text-justify}

> **MACD** (Moving Average Convergence Divergence)
{: .text-justify}

* **Reporting par email**:  L'application peut envoyer automatiquement et de manière quotidienne, hebdomadaire ou mensuelle un rapport complet par email sur une paire de cryptomonnaie, incluant une analyse complète avec les graphiques en pièce jointe.
{: .text-justify}

* **Monitoring**: L'application permet de consulter la température interne du Raspberri pi, l'occupation des CPUs, RAM et espace Disque.
{: .text-justify}

* **Automatisation**: Le système est conçu pour être démarré automatiquement au démarrage du raspberry pi.
{: .text-justify}

{% include gallery id="gallery_graphique" caption="Cliquez pour agrandir l'image." %}

### Stack Technique

**Langage** : Python 3, avec une approche orientée objet pour structurer le code.
{: .text-justify}
**Framework Web** : Flask, un framework web utilisé pour créer l'interface web de l'application.
{: .text-justify}
**Base de Données** : SQLite, un moteur de base de données léger, utilisé pour stocker l'ordonnancement des reportings envoyés par e-mail.
{: .text-justify}
**Bibliothèques** Python Principales :
{: .text-justify}

* **Flask** pour gérer le front-end en python.
{: .text-justify}  
* **python-binance** pour récupérer les données de marché des cryptomonnaies.
{: .text-justify}
* **polars** pour la manipulation et l'analyse des données.
{: .text-justify}
* **plotly** pour la génération de graphiques dynamiques
{: .text-justify}
* **mplfinance** pour la création des graphiques financiers attachés aux mails.
{: .text-justify}
* **schedule** pour gérer l'ordonnanceur de génération des rapports.
{: .text-justify}

## Préparation du Raspberry pi

### Installation de l'OS Headless 64-bits

Ce serveur va fonctionner avec [une version d'OS sans interface graphique](https://www.raspberrypi.com/software/) (Headless). A l'heure où j'écris et article, j'ai installé l'OS **Raspberry PI OS Lite 64Bits** à l'aide du software **Rasbperry Pi Imager** depuis mon ordinateur personnel qui fonctionne sous Linux Mint. J'ai configuré ma Box internet en DNS dynamique pour qu'elle attribue toujours la même adresse IP en local au Raspberry pi, que j'accède en SSH depuis mon ordinateur personnel pour installer et configurer l'application RasCrypto.
{: .text-justify}
J'ai opté pour une configuration fanless avec un [boîtier ArgonOne M2 et un disque SSD Nvme](https://papsdroidfr.github.io/configuration/ArgonOneM2/) pour installer le système: l'installation de l'OS, les reboots et redémarrages du Raspberry pi sont incomparablement plus rapides (reboot + démarrage en 10 secondes) qu'avec une carte SD. Le boîtier ArgonOne sert de refroidissement passif au CPU, il n'atteint jamais la température qui déclenche le petit ventilateur (constatation après 1 mois de fonctionnement h24 7j sur 7): c'est en cela que je le considère "fanless" alors qu'il y a bien un ventilateur pour le protéger au cas où les températures s'emballent.
{: .text-justify}

Pour connaître **l'adresse IP** du Raspberry pi, utilisez cette commande et repérez dans la section wlan0 l'adresse inet qui ressemble à **192.168.xx.yy**: notez la dans un coin car le serveur WEB sera accessible à cette adresse (via le port 5000).
{: .text-justify}

```bash
ifconfig
```

### Installation de l'Application RasCrypto

#### Mise à jour système

Le système étant fraîchement installé, il faut d'abord le mettre à jour et en profiter pour installer les bibliothèques qui vont nous permettre de créer un environnement virtuel, afin de gagner un temps précieux pour installer toutes les dépendances de l'application.
{: .text-justify}
```bash
sudo apt update
sudo apt full-upgrade -y
sudo apt install -y python3-pip python3-venv git
```

#### Création d'un environnement virtuel

Ces étapes doivent être réalisées depuis votre session SSH sur le Raspberry Pi. Nous allons installer le code **dans le répertoire personnel** de l'utilisateur (/home/pi/ si je prends comme hypothèse que vous avez opté pour un utilisateur 'pi').Il est crucial d'utiliser un environnement virtuel pour isoler les dépendances du projet.
{: .text-justify}

```bash
# Assurez-vous d'être dans le répertoire personnel
cd ~

# Créez l'environnement virtuel nommé .venv
python3 -m venv .venv

# Activez l'environnement virtuel
source .venv/bin/activate
```

> Votre invite de commande devrait maintenant être préfixée par (.venv). **Toutes les commandes suivantes doivent être exécutées avec l'environnement activé.**
{: .text-justify}

#### Clonage du Code depuis GitHub
![git](/assets/images/logos/github-flat-shadow-octocat-emblem-64.png){: .align-left}
Clonez le dépôt Git directement dans votre répertoire personnel.
{: .text-justify}
```bash
# Assurez-vous d'être dans le répertoire personnel
cd ~

# Clonez le projet
git clone https://github.com/papsdroidfr/rascrypto.git
```

Ceci va créer un dossier **/rascrypto** contenant les sous-dossiers /cotation et /demo.
{: .text-justify}
* le dossier **/rascrpyto/cotation** contient tout le code de l'application RasCrypto.
{: .text-justify}
* Le dossier **/rascrypto/demo** contient le code d'une démo d'un serveur Web local dont j'ai écrit [cet article](https://papsdroidfr.github.io/dev/webserver/) si vous souhaitez voir un exemple simple de Serveur Web Local en Python avec Flask.
{: .text-justify}

#### Installation des dépendances
Grâce à l'environnement virtuel, l'installation de toutes les bibliothèques Python requises pour le projet est très simple:
{: .text-justify}
```bash
# Déplacez-vous dans le répertoire du code
cd ~/rascrypto/cotation

# Installez les paquets listés dans requirements.txt
pip install -r requirements.txt
```

#### Configuration de l'envoi d'email

L'application utilise un **compte Gmail** pour envoyer les rapports. Pour des raisons de sécurité, vous devez générer un mot de passe d'application spécifique.
{: .text-justify}

Activer la **validation en deux étapes** : La création d'un mot de passe d'application requiert que la validation en deux étapes soit activée sur votre compte Google. Si ce n'est pas le cas, activez-la depuis les paramètres de sécurité de votre compte Google.
{: .text-justify}

**Générer le mot de passe** :

* Allez sur la [page de gestion](https://myaccount.google.com) de votre compte Google.
{: .text-justify}
* Dans le menu de gauche, allez dans la section Sécurité.
{: .text-justify}
* Descendez jusqu'à la section "Comment vous vous connectez à Google" et cliquez sur Mots de passe des applications. (Vous devrez peut-être vous reconnecter).
{: .text-justify}
* Dans le menu déroulant "Sélectionner une application", choisissez Autre (Nom personnalisé).
{: .text-justify}
* Donnez-lui un nom explicite, par exemple RasCrypto, et cliquez sur GÉNÉRER.
{: .text-justify}
* Un mot de passe de 16 caractères sera affiché. **Copiez ce mot de passe immédiatement**. Vous ne pourrez pas le revoir. C'est ce mot de passe que vous utiliserez dans le script, et non le mot de passe de votre compte Google.
{: .text-justify}
  
#### Configuration du script de démarrage

Le script **start.sh** est le point d'entrée qui lance l'application. Vous devez le modifier pour y insérer vos informations personnelles. L'utilitaire nano va permettre de modifier ce fichier.
{: .text-justify}
```bash
nano ~/rascrypto/cotation/start.sh
```

Modifiez les variables suivantes du fichier :
{: .text-justify}
* **export GMAIL_USER**: Remplacez "MONEMAIL@GMAIL.COM" par votre adresse email Gmail qui va envoyer les rapports. Conservez bien les guilements.
* **export GMAIL_APP_PASS**: Remplacez "MON PASSWORD APPPLICATIF GMAIL" par le mot de passe d'application de 16 caractères que vous venez de générer. conservez bien les guillemets.

Ctrl O pour sauvegarder (puis ENTREE) et Ctrl X pour sortir de nano.
{: .text-justify}

Rendez ensuite ce script **exécutable**:
{: .text-justify}
```bash
chmod +x ~/rascrypto/cotation/start.sh
```

#### Configuration de l'application

le fichier **info.py** contient les paramètres principaux de l'application que vous pouvez ajuster en utilisant nano comme précédemment.
{: .text-justify}

* TIME_SCHEDULEUR est l'heure à laquelle les reports sont générés et envoyés par e-mail.
{: .text-justify}
* PERIOD_SHORT est le nombre de périodes pour le calcul de la moyenne mobile court terme.
{: .text-justify}
* PERIOD_LONG est le nombre de périodes pour le calcul de la moyenne mobile long terme.
{: .text-justify}
* PERIOD_SUPPORT est le nombre de périodes qui servent à calculer les niveaux de résistances et de support.
{: .text-justify}
* CRYPTO est le dictionnaire des paires de cryptomonnaies que vous souhaitez suivre et qui vont apparaître dans le menu déroulant du dashboard principal de l'application.
{: .text-justify}

#### Intégration dans Crontab

Pour un **démarrage automatique**, nous allons le préciser dans la crontab du Raspberry pi. Il faut aussi régulièrement rebooter le server, nous allons le programmer pour un reboot hebdomadaire (samedi à minuit)
{: .text-justify}

```bash
sudo nano /etc/crontab -e
```

ajoutez ces 2 lignes à la fin (en supposant que 'pi' est l'utilisateur configuré sur le Raspberry pi):
{: .text-justify}

```bash
@weekly root reboot
@reboot pi ~/rascrypto/cotation/start.sh &
```

Ctrl O pour sauvegarder (puis ENTREE) et Ctrl X pour sortir de nano
{: .text-justify}

> L'application est maintenant configurée pour s'exécuter automatiquement à chaque redémarrage du Raspberry Pi. Le serveur va redémarrer tous les samedi à minuit.
{: .text-justify}

Pour redémarrer le Raspberry pi
{: .text-justify}

```bash
sudo reboot
```

> Il est important d'avoir noté l'adresse IP **192.168.xx.yy** du Raspberry pi et de s'assurer que votre Box internet vous attribuera toujours la même adresse IP. Le serveur WEB local généré par l'application RasCrypto est alors accessible sur le port 5000 depuis l'adresse http://192.168.xx.yy:5000, en vous connectant sur le même réseau que le Raspberry pi (non accessible en dehors de votre réseau local).
{: .text-justify}

> Compte tenu de **l'absence de sécurité** mise en oeuvre pour protéger le réseau des menaces externes (il faudrait ajouter un serveur dédié pour la base de données, un serveur reverse proxy et un serveur WYSIWIG qui fait tampon avec le serveur applicatif, ainsi que protéger le mot de passe applicatif via un cryptage): je vous **déconseille fortement** de "bidouiller" ce projet pour le rendre accessible depuis l'extérieur. Il n'est pas du tout configuré pour, et vous exposeriez votre propre réseau interne à toutes les menaces du WEB où une grande majorité de bots malveillants viendraient repérer les failles de sécurité: ils se régaleraient dans ces conditions.
{: .text-justify}

## Structure Logicielle et Modèle de Classes

Le code est structuré de manière modulaire et orientée objet pour séparer les responsabilités.
{: .text-justify}

### appl.py

C'est le point d'entrée principal de l'application. Il contient la classe **SiteWebLocal** qui défini tout le front-end exécuté en tâche de fond, tandis que le thread principal faisant office de scheduler contrôle s'il faut générer et envoyer les reports par emails.
{: .text-justify}

### cotation.py

Contient la classe **BinanceAPI**. Elle va permettre de récupérer les données de marché.
{: .text-justify}

* **Responsabilité** : Récupère les données brutes du marché pour une parie de cryptomonnaie (exple: BTCUSDC)
{: .text-justify}
* **Méthodes** : Elle contient une méthode qui récupère dans un dataframe polars l'historiques des données **OHLCV** (**O**pen price, **H**igh price, **L**ow price, **C**lose price, **V**olume) sur une profondeur d'historique donnée (start date) et des intervalles définis (exple 4h, 1h ...)
{: .text-justify}

### analysis.py

Contient la classe **TechnicalChartBuilder**. C'est le cœur de l'analyse technique.
{: .text-justify}

* **Responsabilité** : Construction d'analyses techniques modulaires
{: .text-justify}
* **Méthodes** : Elle contient les méthodes pour calculer tous les indicateurs techniques qui sont ajouté au dataframe polars qui contient l'historique des cours (statistiques de performance, moyennes mobiles, oscillateurs RSI et MACD, générateur de graphique exporté en png.)
{: .text-justify}

### emailer.py 

Contient la classe **SimpleEmailer** utilisée pour envoyer des emails avec pièces jointes.
{: .text-justify}

* **Responsabilité** : Gérer l'envoi des emails.
{: .text-justify}
* **Méthodes** : fabrique le corps d'un email au format HTML et envoie l'email à un destinataire avec des images en pièce jointe.
{: .text-justify}

### scheduleur.py

Contient la classe **JobStore** qui gère la base de données pour les tâches planifiées du scheduler.
{: .text-justify}

* **Responsabilité** : Gérer les interactions avec la base de données SQLite.
{: .text-justify}
* **Méthodes** : Créer les tables si elles n'existent pas, insérer les nouvelles données de reports à produire, les lister et éventuellement les supprimer.
{: .text-justify}

## Détail des Analyses Techniques Proposées

L'application calcule et affiche plusieurs indicateurs d'analyse technique standards.
{: .text-justify}

### Statistiques basiques

Pour une période choisie (par exemple 1 mois), l'application indique le cours de clôture à l'ouverture de la période, le cours de clôture à la fin de la période, la différence et la variation en %.
{: .text-justify}

![rascrypto](/assets/images/tutos/043Rascrypto/stats.png){: .align-center}

### Affichage OHLCV

Le graphique de base est un graphique en chandeliers japonais. Chaque "bougie" représente un intervalle qui dépend de la profondeur choisie (5mn pour 1 journée, 1h pour 1 semaine, 4h pour 1 mois, 1 jour pour 1an, 1 semaine pour 5 ans) et affiche 4 informations clés.
{: .text-justify}

![rascrypto](/assets/images/tutos/043Rascrypto/chandeliers.png){: .align-center}

> Open : Cours à l'ouverture de la période.
{: .text-justify}
> High : Cours le plus haut de la période.
{: .text-justify}
> Low : Cours le plus bas de la période.
{: .text-justify}
> Close : Cours à la clôture de la période.
{: .text-justify}
> Le volume des transactions de chaque période est affiché sous forme d'histogramme en bas du graphique.
{: .text-justify}

Pour un confort de lecture, les chandelles sont vertes quand le cours progresse (close > open) et rouge dans le cas contraire, bien que l'information soit visible sur le graphique sans les couleurs.
{: .text-justify}

### Moyennes Mobiles

Il s'agit d'indicateurs de tendance. Une moyenne mobile lisse les données de prix pour identifier la direction de la tendance. Une moyenne mobile sur N périodes est la moyenne du cours de clôture des N dernières périodes.
{: .text-justify}
L'application calcule et affiche deux moyennes mobiles :
{: .text-justify}

* Une courte (ex: 7 périodes) pour la tendance à court terme.
{: .text-justify}
* Une longue (ex: 20 périodes) pour la tendance de fond.
{: .text-justify}

![rascrypto](/assets/images/tutos/043Rascrypto/moyenne-mobile.png){: .align-center}

### Supports et Résistances

Ces niveaux sont des "planchers" (supports) et des "plafonds" (résistances) psychologiques où les prix ont tendance à stagner ou à rebondir. L'application identifie ces niveaux en détectant les points bas (minima locaux) et les points hauts (maxima locaux) significatifs sur une période donnée. Ils sont matérialisés par des lignes horizontales sur le graphique (en haut et en rouge on trouve les résistances, en bas en vert on retrouve les supports).
{: .text-justify}

![rascrypto](/assets/images/tutos/043Rascrypto/supports.png){: .align-center}

### RSI (Relative Strength Index)

Il s'agit d'un indicateur de prédiction. C'est un oscillateur qui mesure la force et la vitesse des mouvements de prix. Il compare l'ampleur des hausses récentes à celle des baisses récentes. Le résultat est un indice oscillant entre 0 et 100. Un RSI au-dessus de 70 est souvent considéré comme un signal de "sur-achat" (le prix a monté trop vite), tandis qu'un RSI en dessous de 30 est considéré comme un signal de "sur-vente". Les prix ont tendance à changer de direction dans ces cas.
{: .text-justify}

![rascrypto](/assets/images/tutos/043Rascrypto/rsi.png){: .align-center}

Cet indicateur est positionné en dessous du graphe avec les chandeliers, sur la même échelle de temps.
{: .text-justify}

### MACD (Moving Average Convergence Divergence)

C'est encore un indicateur de prédiction de type oscillateur. Il montre la relation entre deux moyennes mobiles exponentielles (MME) du prix. Une moyenne mobile exponentielle va faire un calcul de moyenne pondérée au lieu de faire la moyenne des cours sur une période. Le MACD est calculé en soustrayant la MME longue (ex: 26 jours) de la MME courte (ex: 12 jours). Une "ligne de signal", qui est une MME du MACD lui-même (ex: 9 jours), est également tracée.
{: .text-justify}

![rascrypto](/assets/images/tutos/043Rascrypto/MACD.png){: .align-center}

L'histogramme du MACD (la différence entre les deux lignes) visualise l'élan de la tendance, tandis que le croisement de la ligne MACD avec sa ligne de signal peut prédire une forte probabilité à un retournement de situation.
{: .text-justify}

## Reporting automatiques par e-mail

L'application permet de planifier des envois de reports par e-mail tous les jours, toutes les semaines, tous les mois. Il suffit de s'abonner à un topic de l'onglet "Planification Emails" en précisant la paire de cryptomonnaie et la fréquence souhaitée.
{: .text-justify}

![rascrypto](/assets/images/tutos/043Rascrypto/reports.png){: .align-center}

> à ce jour j'ai codé un report unique quelque soit la fréquence choisie (je ferai un update plus tard): le report génère une analyse technique sur un mois, et une autre sur une semaine, regroupant les 2 analyses dans un seul mail.
{: .text-justify}

![rascrypto](/assets/images/tutos/043Rascrypto/mail30j.png){: .align-center}
![rascrypto](/assets/images/tutos/043Rascrypto/mail7j.png){: .align-center}

> Et en plus il est poli!
{: .text-justify}

## Monitoring serveur

Enfin dernière touche personnelle sur ce serveur: une page de monitoring de la température interne, utilisation de la RAM, utilisation des CPU et espace disque. Les données sont rafraîchies toutes les 5 secondes.
{: .text-justify}

![rascrypto](/assets/images/tutos/043Rascrypto/monitoring.png){: .align-center}

> Cet monitoring "temps réel" ne permet pas de voir ce qu'il se passe avec la RAM et les CPU lorsque le dashboard calcule et affiche les analyses techniques. Une évolution consisterai à sauvegarder en base de données les captures pour les afficher ensuite sous forme de graphe sur cette page. Cependant les calculs sont très rapides en polars, il faudrait capter les données plusieurs fois par secondes ce qui alourdirai à mon humble avis le travail de ce serveur. En attendant, vous pouvez aller regarder via 'htop' ce qu'il se passe en vous connectant en SSH sur le Raspberry pi.
{: .text-justify}
