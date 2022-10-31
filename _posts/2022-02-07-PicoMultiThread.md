---
#source: https://www.papsdroid.fr/post/multi-thread-pico
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
  teaser : "/assets/images/tutos/029PicoMultiThread/HardwarePICO-300.png"

#SEO tags
title       : "Programmation multi-thread Raspberry PICO"
image       : "/assets/images/tutos/029PicoMultiThread/HardwarePICO-300.png"
description : "Une des caractéristiques très intéressante du Raspberry PICO est qu'il est composé d'un double coeur ARM Cortex-M0+ @133MHz. Cela signifie qu'il y a deux processeurs indépendants..."
toc: true
toc_sticky  : true
toc_label   : "Multi-thread"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "DEV" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "pybstick" "python3" "micro-pyhton" "électronique"
tags        : ["raspberry pico", "multi-thread", "micro-python"]

---

![PicoMultiThread](/assets/images/tutos/029PicoMultiThread/HardwarePICO-300.png){: .align-left}
Une des caractéristiques très intéressante du [Raspberry PICO](https://datasheets.raspberrypi.com/rp2040/hardware-design-with-rp2040.pdf) est qu'il est composé d'un **double coeur** ARM Cortex-M0+ @ 133MHz. Cela signifie qu'il y a deux processeurs indépendants qui se partagent la mémoire et les périphériques. La programmation **multi-thread** va permettre de paralléliser des traitements en exploitant les deux cœurs, je vais vous montrer un exemple très simple en micro-python.
{: .text-justify}

## Pourquoi faire du multi-thread ?
On va parler de **multi-thread** lorsqu'au moins 2 processus peuvent s'exécuter en parallèle. Ils peuvent très bien s’exécuter sur un même cœur (la charge du cœur étant partagée par les threads exécutés sur ce même cœur), ou bien sur des cœurs distincts (dans ce cas ils ne partagent que les ressources communes tels que mémoire, périphériques etc... mais chaque thread dispose alors de toute la puissance de son cœur dédié).
{: .text-justify}
Il y a beaucoup de situations où on a besoin de paralléliser des traitements. Je ne vais pas toutes les énumérer mais voici les exemples les plus récurrents avec un projet sur microcontrôleur où exploiter plusieurs cœurs est très intéressant:
{: .text-justify}
* Cas de calculs un peu complexes à réaliser qui peuvent être parallélisés: autant répartir la charge de calcul sur les 2 cœurs dans ce cas. 
{: .text-justify}
* Dans une Interface Homme-Machine (IHM), pour fluidifier l'expérience utilisateur il est très pratique de réserver la capture des commandes utilisateur (click sur un bouton par exple) dans un thread et l'interprétation des commandes (affichage écran, déclenchement d'un son, activation d'un moteur etc ...) dans d'autres threads distincts: de cette façon le thread de capture des commande utilisateur n'est pas saturé ou ralenti par les interprétations, plus particulièrement s'il est exécuté sur un cœur dédié.
{: .text-justify}
* Dans un projet domotique ou avec des capteurs, tout comme avec une IHM, il est bien avisé de gérer la capture des informations fournies par les capteurs dans un thread dédié, et d'exploiter les résultats dans un autre thread indépendant. Dans le cas contraire l'exploitation des données capturées est fortement perturbée par le temps mis pour capter les informations. 
{: .text-justify}

## Exemple de programme multi-thread mono-cœur

>Ce programme en micro-python va lancer un compte à rebours exprimé en secondes, afficher le temps restant toutes les secondes en faisant clignoter la led interne du PICO.
{: .text-justify}
```python
import machine, time
from machine import Timer

#led interne du raspberry pico
pico_led = machine.Pin(25, machine.Pin.OUT) 

# Objet compte à rebours
class CptRebours():
    #constructeur, initialise timeleft
    def __init__(self, t=0):
        self.timeleft = t             
    # fct callbackappelée par le timer
    def countdown(self, tm):    
        if self.timeleft > 0 :
            self.timeleft -= 1
            
#initialisation d'un objet compte à rebours    
cptr = CptRebours() 

#boucle du core principal
while True:
    cptr = CptRebours(int(input("Compte à rebours en secondes: ")))
    print('Démarrage compte à rebours ...')
    tim = Timer(period=1000, callback=cptr.countdown)
    
    while (cptr.timeleft > 0 ):
        pico_led.toggle()
        print('temps restant:', cptr.timeleft, 'secondes')
        time.sleep(1)
    
    print("BIP BIP BIP compte à rebours terminé !")
    tim.deinit() #stoppe et libère le timer
```

Pour gérer le compte à rebours j'utilise un **Timer** qui va toutes les 1000ms (1 seconde) exécuter par **interruption logicielle** la fonction callback countdown() d'un objet CptRebours.
{: .text-justify}
Comprenez bien qu'il s'agit d'un traitement **multi-thread par interruption logicielle**: le Timer est bien géré en parallèle de ma boucle principale. Mais tout est géré dans un seul cœur puisqu'à aucun moment nous n'avons configuré l'exécution d'un thread dans l'autre cœur qui reste totalement inerte.
{: .text-justify}
Si vous exécutez ce programme, vous verrez s'afficher chaque seconde le temps qu'il reste, et la led s'allume et s’éteint à chaque affichage: tout est parfaitement synchronisé.
{: .text-justify}
Si maintenant on souhaite que le clignotement de la led ne soit pas synchronisé avec l'affichage des secondes restantes: par exemple qu'elle clignote toutes les 0.36 secondes! Une méthode vraiment moche consisterait à lancer et arrêter un second Timer directement dans l'objet CptRebours pour faire clignoter cette led. C'est très moche (coding bourrin) car on va venir faire clignoter une led dans une méthode prévue pour décrémenter le compte à rebours. Mais si je veux aussi que ma led clignote d'une certaine façon et accélère le clignotement quand il ne reste que 5 secondes ? Pareil on pourrait ajouter un troisième Timer et rendre le code encore plus laid et complexe en continuant sur cette très mauvaise idée.
{: .text-justify}
>Vous l'aurez compris: on souhaite que l'animation de la led soit **indépendante** de l'affichage du compte à rebours, c'est l'occasion rêvée de passer ce traitement dans un cœur dédié.
{: .text-justify}

## Programme multi-thread double-cœur
Nous allons donc supprimer le clignotement de la led de la boucle principale et le confier à un thread qui sera exécuté sur l'autre cœur du raspberry PICO.
{: .text-justify}
```python
import machine, time, _thread
from machine import Timer

#led interne du raspberry pico
pico_led = machine.Pin(25, machine.Pin.OUT) 

# Objet compte à rebours
class CptRebours():
    #constructeur, initialise timeleft
    def __init__(self, t=0):
        self.timeleft = t
    # fct callbackappelée par le timer
    def countdown(self, tm):    
        if self.timeleft > 0 :
            self.timeleft -= 1
        
# Fonction exécutée dans le second thread 
# gère le clignotement de la led
def thread_anim():
    while True:
        if (cptr.timeleft>0):
            # fait clignoter la led interne
            pico_led.toggle()
            # attente supplémentaire s'il reste plus de 5s
            if cptr.timeleft>5:  
                time.sleep(0.18)                
        else:
            # extinction de la led interne
            pico_led.value(0)
        # temps d'attente clignotement
        time.sleep(0.18)

#initialisation d'un compte à rebours
cptr = CptRebours() 

#démarrage du thread d'animation
_thread.start_new_thread(thread_anim, ()) 

#boucle du thread principal
while True:
    cptr = CptRebours(int(input("Compte à rebours en secondes: ")))
    print('Démarrage compte à rebours ...')
    tim = Timer(period=1000, callback=cptr.countdown)
    
    while (cptr.timeleft > 0 ):
        print('temps restant:', cptr.timeleft, ' secondes')
        time.sleep(1)
    
    print("BIP BIP BIP compte à rebours terminé !")
    tim.deinit() #stoppe et libère le timer
```

Il faut importer la bibliothèque **_thread** dans un premier temps (1ère ligne du programme)
{: .text-justify}
Vous pouvez constater que la définition de la classe CptRebours n'a pas bougée d'un iota.
{: .text-justify}
Ensuite j'ai défini une nouvelle fonction **thread_anim()** qui gère le clignotement de la led interne du PICO en fonction de l'état du compte à rebours. Le code est très simple à comprendre: une boucle infinie va faire clignoter la led avec un temps d'attente de 2 fois 0.18s=0.36s s'il reste plus de 5 secondes au compte à rebours, sinon un temps d'attente de 0.18s s'il reste moins de 5 secondes, et enfin laisse la led éteinte si le compte à rebours est à zéro.
{: .text-justify}
De cette façon je peux gérer mon algorithme de clignotement de led de façon indépendante, sans venir "pourrir" ou complexifier le code de ma classe "CptRebours".
{: .text-justify}
Pour exécuter cette fonction **thread_anim()** dans un thread sur le **second cœur**: rien de plus simple il suffit d'exécuter ce code, et dès que le compte à rebours est initialisé avec une valeur différente de 0, hop la led clignote exactement comme on le souhaite !
{: .text-justify}

```python
_thread.start_new_thread(thread_anim, ()) 
```

Vous remarquerez ensuite que le corps de la boucle du cœur principal n'a pas bougé, a l’exception du pico_led.toggle() qui a bien évidemment été retiré.
{: .text-justify}

>Attention si l'on souhaite maintenant exécuter une autre fonction avec la méthode _thread.start_new_thread **ça va planter**: le PICO vous répondra que ce cœur est déjà utilisé! Il faut dans ce cas complexifier un peu le programme avec la technique des **sémaphores** bien implémentée dans la bibliothèque _thread, mais je trouve que ça dépasse un peu les capacités d'un micro-contrôleur. Si on en vient à devoir répartir la charge de plusieurs threads avec des sémaphores sur un même cœur, à mon avis il faut plutôt utiliser un Raspberry pi multi-core ou une machine plus adaptée.
{: .text-justify}

Pour utiliser des sémaphores, il faut commencer par **allouer un token**:
{: .text-justify}
```python
token=_thread.allocate_lock()
```

Ensuite, il faut indiquer à l'intérieur de la fonction exécutée par un thread qu'elle souhaite **acquérir le token** pour monopoliser le cœur. Le token lui sera attribué dès lors qu'il est libéré.
{: .text-justify}
```python
token.acquire( waitflag=0)
```

Ne pas oublier de **libérer le token** à la fin du code. 
{: .text-justify}
```python
token.release()
```

Ainsi, les thread exécutés sur le même cœur "attendent" que le token soit libéré pour pouvoir exécuter leur code. Il faut s'assurer que le code exécuté qui monopolise le token ne soit pas trop long, car les autres thread vont attendre qu'il se libère !
{: .text-justify} 
