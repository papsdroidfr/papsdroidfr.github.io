---
#source: https://www.papsdroid.fr/post/jeux-pygame-partie04
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
  teaser : "/assets/images/tutos/035Pygame/pygame06.png"

#SEO tags
title       : "Coder un jeux avec Pygame - 04 animation des agents"
image       : "/assets/images/tutos/035Pygame/pygame06.png"
description : "Dans cette quatrième partie du tutoriel apprendre à coder un jeux avec Pygame, nous allons voir comment gérer les déplacements de nos agents sur le terrain."

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Pygame"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "DEV" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["python3", "Pygame" ]

---

![Pygame](/assets/images/tutos/035Pygame/pygame06.png){: .align-left}
Dans cette quatrième partie du tutoriel [apprendre à coder un jeux avec Pygame](https://www.papsdroid.fr/post/jeux-pygame), nous allons voir comment **gérer les déplacements** de nos agents sur le terrain. Chaque agent (un tank, un obus et une explosion de l'obus) va être un objet qui se déplace en suivant des lois de physique en lien avec le jeux. Les interactions entre les agents et le décors seront vues plus tard dans un autre tutoriel, ce qui explique pourquoi pour le moment notre petit tank peut traverser les murs, ainsi que les obus qu'il tire. Nous allons pouvoir gérer des déplacements et animations parfaitement fluides, même depuis un Raspberry pi, comme le montre la vidéo ci-dessous.
{: .text-justify}

{% include video id="EDCy42aayH4" provider="youtube" %}

## Pré-requis:
* Si vous prenez ce tuto encours de route, il faut commencer par [les bases avec Pygame](https://www.papsdroid.fr/post/jeux-pygame).
{: .text-justify}
* Pour la création des décors en background , c'est expliqué dans [ce billet](https://www.papsdroid.fr/post/jeux-pygame-partie02).
{: .text-justify}
* Animation avec rotation des agents tank: [par ici](https://www.papsdroid.fr/post/jeux-pygame-partie03).
{: .text-justify}


## Un peu de physique concernant les déplacements
Tout déplacement va subir des **contraintes mécaniques** qui vont permettre de simuler des déplacements "réalistes". Par exemple pour un jeux de plateau où l'on saute sur des plateformes, il faut mettre en oeuvre une loi de gravitation qui fait "tomber" naturellement les objets qui sont dans le vide. Dans notre cas nous allons plutôt simuler des règles qui régissent la **vitesse** , **l'accélération** et les **frottements** de chaque agent.
{: .text-justify}

### Unités de mesure
La vitesse c'est la distance que parcourt un agent par unité de temps. Dans notre monde à nous, on l'exprime en km/h: nombre de kilomètres parcourus par heure. Ces unités (km et heure) ne sont pas du tout pratiques pour notre jeux! Nos **distances** s’expriment en **nombre de pixels**, et pour l'unité de temps on va se baser sur le temps entre l'affichage de deux frames (images) consécutives: **le FPS** (Frame par secondes) correspond aux nombre d'images affichées par secondes (FPS = 30 signifie que le jeux affiche 30 images par secondes, ou encore qu'il se passe 1/30ème de secondes entre 2 images). Notre unité de temps va être: 1/FPS secondes. 
{: .text-justify}

### Mesures et lois physiques
* **Unité de temps**: 1/FPS secondes où FPS est le nombre de Frames (images) par Secondes.
{: .text-justify}
* **Distance** parcourue: nombre de pixels.
{: .text-justify}
* **Vitesse**: nombre de pixels parcourus par unité de temps. Par exemple une vitesse de 10 signifie que notre agent va parcourir 10 pixels entre chaque frame, dans la direction où il est orienté. Une vitesse peut être négative et dans ce cas l'agent "recule" au lieu d'avancer. Si sa vitesse est nulle: il reste figé sur place.
{: .text-justify}
* **Accélération**: changement de vitesse par unité de temps. une accélération de +1 signifie que la vitesse gagne +1 à chaque nouvelle frame. Une accélération peut très bien être négative et dans ce cas la vitesse diminue entre chaque frame. Si l'accélération est nulle, alors la vitesse de l'agent reste constante.
{: .text-justify}
* **Frottement**: il s'agit d'une accélération négative qui provoque le ralentissement naturel d'un agent jusqu'à ce que sa vitesse devienne nulle, auquel cas le frottement ne s'applique plus.
{: .text-justify}
>Rien de bien compliqué à coder quand on raisonne avec les bonnes unités: ça devrait vous rappeler quelques souvenir de physique au lycée ;-)
{: .text-justify}

## Objets agents
![Pygame](/assets/images/tutos/035Pygame/posTank.png){: .align-left}
Chaque agent que l'on va animer est défini par les variables et méthodes suivantes: 
{: .text-justify}
* **un nombre de rotations prédéfini** pour pré-calculer toutes les rotations à l'avance (voir le tutoriel n°3)
{: .text-justify}
* **des listes pré-calculées de sinus et cosinus pour chaque rotation**: nos amis sinus et cosinus vont nous aider à diriger le tank dans la bonne direction. En effet si la vitesse du tank est v, sa position centrale est [x0,y0], et son orientation l'angle α (exprimé selon la bibliothèque PIL dans le sens inverse des aiguilles d'une montre, angle 0 correspond à midi), alors sa prochaine position sera: [x0-v.sin(α),y0-v.cos(α)]. Les précalculs des sinus et cosinus vont nous faire gagner un précieux temps CPU au lieu de les recalculer à chaque fois que le tank se déplace ("FPS" fois par secondes...)
{: .text-justify}
* **une liste d'images qui seront affichées en incrustation**, avec possibilité d'activer/désactiver chaque image: cela permet d'une part d'afficher ou non une partie des images de l'agent (exple le bouclier du tank qui ne s'affiche que lorsqu'il est activé) et d'envisager des animations qui consistent à afficher les images les unes après les autres (exemple: l'explosion d'un obus)
{: .text-justify}
* une **position**, une **vitesse**, une **vitesse maximale**, une **accélération** et un **frottement** qui lui est propre et peuvent évoluer. 
{: .text-justify}
* **un timer interne** qui démarre à zéro, et est incrémenté de +1 à chaque fois qu'il bouge (entre 2 frames donc). Ce timer permet d'envisager des animations pour afficher certaines parties en fonction du timer. 
{: .text-justify}
* **une méthode qui le fait bouger** entre 2 frames selon les lois de physique exposées plus haut en modifiant en conséquence ses positions, vitesse, accélération
{: .text-justify}
* **une méthode qui l'oriente** (en entier ou une partie) avec un angle
{: .text-justify}
* **une méthode qui le dessine** sur son terrain.
{: .text-justify}

### objet Tank
![Pygame](/assets/images/tutos/035Pygame/tank_rotation.png){: .align-left}
Un tank est un agent composé de 3 images (**corps**, **canon**, **bouclier**) toutes orientables. Il possède une vitesse max de 10, et un frottement de -1. Il est défini par:
{: .text-justify}
* un agent obus qu'il sera prêt à tirer
{: .text-justify}
* une méthode qui le fait bouger: accélération (flèches haut et bas), rotation du tank (flèches gauche et droite), rotation de la tourelle( lettres "s" ou "d") selon qu'il est contrôlé au clavier ou non
{: .text-justify}
* une méthode pour activer / désactiver son bouclier
{: .text-justify}
* une méthode pour tirer un obus
{: .text-justify}

### objet Obus
![Pygame](/assets/images/tutos/035Pygame/tank_obus.png){: .align-left}
Un obus est un agent composé de 2 images (obus + éclair de feu). Il possède une vitesse de 30, une accélération de 0 et un frottement de 0. Il est défini par:
{: .text-justify}
* Le tank qui l'a tiré
{: .text-justify}
* un agent explosion qui sera activé quand la vitesse de l'obus est à 0
{: .text-justify}
* un compte à rebours: s'il vaut zéro c'est que l'obus n'a pas encore tiré, sinon il a été tiré et il faut attendre que le compte à rebours soit à nouveau à zéro avant que le tank ne puisse retirer.
{: .text-justify}
* une méthode qui le fait bouger en activant au 1er timer un éclair de feu positionné et orienté comme le canon du tank qui l'a tiré, et qui est désactivée donc lors des timer suivants (effet explosif au niveau de la sortie du canon), et qui génère une explosion si la vitesse de l'obus est 0.
{: .text-justify}

### Objet explosion
![Pygame](/assets/images/tutos/035Pygame/explosion.png){: .align-center}
Une explosion est un agent qui possède 9 images, une vitesse de 0, une accélération de 0 et qui est défini par:
{: .text-justify}
* l'obus qui va générer cette explosion
{: .text-justify}
* un indicateur pour activer l'explosion: tant qu'il n'est pas actif les images sont toutes désactivées par défaut et l'agent ne bouge pas ni n'est dessiné sur le terrain.
{: .text-justify}
* une méthode pour bouger: si l'indicateur d'activation de l'explosion est activé, alors les images sont activées/désactivées à tour de rôle là où l'obus s'est arrêté, en fonction du timer interne de l'agent pour générer une animation.
{: .text-justify}
> Nous avons tous les éléments conceptuels pour décrire notre classe Agent et les classes Tank, Obus, Explosion qui vont toutes hériter de la classe Agent.
{: .text-justify}

## Les programmes Python3
Ils sont disponible, avec toutes les images (libre de droit) [sur mon Github](https://github.com/papsdroidfr/Tuto_Pygame_Tank).
{: .text-justify}
Téléchargez bien tout (bouton vert à droite "Clone or Download) il faut l'intégralité du projet Tank04 avec tous les dossiers et sous-dossiers qui contiennent les images libres de droit, récupérées sur le site [craftpix.net](https://craftpix.net/categorys/sprites/) 
{: .text-justify}
Si vous lisez avec attention les explications ci dessus, la lecture et compréhension des programmes devrait être d'autant plus facile que j'y ai ajouté de nombreux commentaires. Les programmes sont découpés selon les classes:
{: .text-justify}
* **class_Terrain.py**: définition de la classe Terrain() qui génère les décors de fonds. Les explications sont détaillées dans le [tutoriel 02](https://www.papsdroid.fr/post/jeux-pygame-partie02).
{: .text-justify}
* **class_Agent.py**: définition de notre classe Agent() qui va pré-calculer toutes les rotations des listes d'images qui compose notre agent (voir le [tutoriel 03](https://www.papsdroid.fr/post/jeux-pygame-partie03) pour les explications), ainsi que les listes associées de sinus et cosinus, et régir toutes les lois de déplacements en tenant compte de sa position, de son orientation, sa vitesse, son accélération et les forces de frottements subies.
{: .text-justify}
* **class_Tank.py**: définitions de nos 3 classes Explosion(Agent), Shell(Agent) et Tank(Agent), qui dérivent donc toutes de la classe Agent(). Toute la conception de ces classes est expliquée plus haut.
{: .text-justify}
* **tank04.py**: il s'agit du programme principal à exécuter . Il est composé d'une classe Game() qui va initialiser le plateau de jeu et les tanks (un seul dans cet exemple). La boucle principale du programme Pygame lit les événements pour réagir aux touches clavier qui déclenchent un événement (Quitter, ESPACE = activer/désactiver le bouclier du tank, "F" = Tirer un obus). Le FPS est limité à 30 par défaut. Ensuite l'affichage est simple: d'abord les tanks, ensuite les éventuels obus s'ils ont été tirés, et les explosions des obus s'ils les explosions sont activées.
{: .text-justify}

>Prochain tuto à venir: la gestion des collisions! à suivre ....
{: .text-justify}