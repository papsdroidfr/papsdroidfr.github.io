---
#source: https://www.papsdroid.fr/post/planetaire-calcul-engrenages
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
  teaser : "/assets/images/tutos/033Planetaire/02_4roues.png"

#SEO tags
title       : "Planétaire: calcul des engrenages"
image       : "/assets/images/tutos/033Planetaire/02_4roues.png"
description : "Calcul des engrenages du planétaire mécanique avec une simulation en Python."

excerpt_separator: <!--more-->
toc: true
toc_sticky  : true
toc_label   : "Engrenages"

# category: "tutoriels" "configuration" "IA" "DEV" "aquapi" "planétaire" 
category    : "DEV" 

# tag: "raspberry pzero" "raspberry pi" "raspberry pico" "PYBStick" "python3" "micro-pyhton" "électronique"
tags        : ["planétaire", "python3" ]

---
![Planétaire](/assets/images/tutos/033Planetaire/02_4roues.png){: .align-left}
Dans [l'article précédent](https://papsdroidfr.github.io/tutoriels/engrenages/), nous avons pu voir le fonctionnement des engrenages avec le **calcul du rapport de réduction**. Nous avons vu qu'un engrenage simple ne permet pas d'aller chercher de gros rapports de réduction car nous sommes limités en nombre de dents, tandis que les engrenages à étages permettent de concevoir des rapports de réduction plus grands et plus précis, mais il faut résoudre une équation assez terrifiante afin de trouver les bonnes roues dentées qui approchent le plus possible un **rapport de réduction R**  souhaité :
{: .text-justify}
```
R = (-1)^y * (Z1*Z2*..*Zn) / (D1*D2*...Dm) 
```
* y est le nombre de contacts entre les dents.
* Zi, Dj le nombre de dents d'une roue.
* n le nombre de roue menantes.
* m le nombre de roue menées.

On va s'aider du langage de programmation **Python** pour trouver la meilleure solution possible à cette équation, et vous verrez qu'on va obtenir des résultats en quelques secondes  avec une précision diabolique ( 0.001% près). 
{: .text-justify}

## Impossible algorithme de force brute
On peut dans un premier temps se dire: tient et si on évalue toutes les combinaisons de roues dentées possibles pour retenir le meilleur résultat ? Limitons nos roues dentées entre 13 dents et 99 dents: nous avons 87 roues dentées possibles. Si je veux envisager 4 engrenages sur 2 étages, il me faut choisir 4 roues menantes et 4 roues menées. Jusque là tout va bien.
{: .text-justify}
Ça va se corser quand on évalue le nombre de combinaisons: choisir 4 roues parmi 87 (les 4 pouvant être identiques) va donner 87⁴ combinaisons = 5,7 millions de combinaisons. Quand on combine les choix des deux étages, on se retrouve avec (87⁴)² , soit plus 3 millions de milliards de combinaisons possibles (le Capitaine Haddock dirait 3 mille milliards de mille sabords de combinaisons !).
{: .text-justify}
**Autant dire qu'on va oublier la force brute**: évaluer une par une autant de combinaisons nous ferait attendre les résultats pendant très très longtemps: on va devoir ruser un peu, et faire des recherches beaucoup plus ciblées.
{: .text-justify}

## Recherches optimisées
### Quelques hypothèses
Tout d'abord on va oublier le terme (-1)^y de l'équation qui donne le sens de rotation de la dernière roue par rapport à la première: nous avons vu qu'il suffit d'insérer n'importe quelle roue dentée entre la dernière et l'avant dernière afin d'inverser le sens de rotation.
{: .text-justify}
Ensuite nous allons raisonner avec des rapports de réduction "multiplicateur" et non pas diviseur: c'est à dire que **R > 1**. Si on veut inverser il suffit tout simplement d'inverser le rôle des roues menées et des roues menantes. Par exemple la terre tourne autour du soleil en 365.256 jours: on va calculer un rapport de réduction R = 365.256 c'est bien plus parlant que de chercher l'inverse 1/R = 0,002737806.
{: .text-justify}
Pour limiter la taille des roues dentées, nous allons nous **limiter à des roues de 13 à 99 dents** max. Par ailleurs puisque nous voulons obtenir un rapport de réduction > 1 nous allons cibler des petites roues sur l'étage du bas (dénominateur de l'équation), et des roues plus grosses sur l'étage du haut (numérateur), ce qui va considérablement diminuer les choix possibles.
{: .text-justify}
* Choix des roues dentées pour l'étage du bas (dénominateur de l'équation): 13 à 17 dents.
{: .text-justify}
* Choix des roues dentées pour l'étage du haut (numérateur de l'équation): 18 à 99 dents.
{: .text-justify}
Autant évaluer des millions de milliards de combinaisons avec un ordinateur standard devient impossible, autant en évaluer plusieurs millions devient tout à fait abordable, et même que ça tourne en quelques secondes sur un Raspberry pi4 quand on sait utiliser les bonnes bibliothèques.
{: .text-justify}

### Recherche ciblée avec Numpy
Le code python est disponible dans le [Github du projet](https://github.com/papsdroidfr/planetaire). Il s'agit du programme **engrenage_reduction.py** dans la section **/python**.
{: .text-justify}
Notre recherche va consister à faire de la **force brute "ciblée"** en utilisant la bibliothèque Numpy sous Python qui permet de vectoriser des tableaux de grande dimension, et de les exploiter très efficacement sous Python.
{: .text-justify}
Dans un premier temps on calcule toutes les multiplications entre les 5 roues possibles du bas (idem avec les 82 roues du haut), en combinant 2, 3 ou 4 roues. Cela nous donne des tableaux multidimensionnels avec au maximum 4.5 millions de résultats pour 4 roues parmi 82 (=82⁴). A ce stade je ne m'embête même pas à essayer d'éliminer les cas identiques dus à la commutativité de la multiplication: j'y passerai beaucoup plus de temps que de calculer 4.5 millions de multiplications. Avec Numpy, le Raspberry pi4 fait ces calculs en quelques secondes, mais il faut **au minimum 4Go de RAM** pour stocker les tableaux en mémoire.
{: .text-justify}
Une fois que j'ai mes tableaux de multiplications toutes évaluées, je vais opter pour 2 modes de recherche: simple et complexe. Dans le mode simple, les petites roues dentées seront toutes identiques, tandis que dans le mode complexe je vais évaluer tous les cas possibles pour retenir le meilleur choix.
{: .text-justify}
Quand j'évalue un choix de petites roues dentées (dénominateur) = D, je sais alors qu'il me faut obtenir un numérateur N = R * D pour obtenir le rapport R voulu. Je vais alors filtrer avec Numpy mon tableau multidimensionnel où j'ai précalculé toutes les multiplications possibles pour ne retenir que les candidats qui me donnent N à 1 pour mille près: en effet rien ne garanti que je vais trouver pile poil la bonne combinaison.
{: .text-justify}
J'évalue alors tous les rapports ainsi ciblés N/D et retient celui qui me donne la meilleure précision en % par rapport à R souhaité. 
{: .text-justify}
La recherche simple (toutes les petites roues sont identiques) donne déjà de très très bons résultats en quelques secondes avec un Raspberry pi4! La recherche complexe quand à elle va durer 2 à 3 mn à peine pour fournir un résultat à peine meilleur (voire identique) que le celui de la solution simple.
{: .text-justify}

#### Résultats obtenus pour la Terre
![Planétaire](/assets/images/tutos/033Planetaire/03RTerreSimple.png){: .align-center}
La terre tourne autour du soleil [en 365,256 363 004 jours](https://fr.m.wikipedia.org/wiki/Orbite_de_la_Terre). La méthode simple me fourni comme résultat:
{: .text-justify}
* **Engrenages du bas**: 16, 16, 16, 16
* **Engrenages du haut**: 40, 82, 82, 89

**R obtenu** = 40x82x82x89 / (16x16x16x16) = 365.256347656, soit une erreur de 0.00004202% !
{: .text-justify}
Si on lance la recherche complexe, on obtiendra la même précision avec comme solution:
{: .text-justify}
* **Engrenages du bas**: 13, 16, 16, 16
* **Engrenages du haut**: 41, 65, 82, 89

**R obtenu** = 41x65x82x89 / (13x16x16x16) = 365.256347656 soit le même résultat qu'avec la solution simple !
{: .text-justify}

#### Résultats obtenus pour la Lune
![Planétaire](/assets/images/tutos/033Planetaire/03RLuneSimple.png){: .align-center}
La Lune tourne autour de la terre [en 27,321 582 jours](https://fr.m.wikipedia.org/wiki/Lune). La méthode simple me fourni comme résultat:
{: .text-justify}
* **Engrenages du bas**: 16, 16, 16
* **Engrenages du haut**: 21, 73, 73

**R obtenu** = 21x73x73 / (16x16x16) = 27,321533203, soit une erreur de 0.00018% !
{: .text-justify}
Si on lance la recherche complexe, on obtiendra comme solution:
{: .text-justify}
* **Engrenages du bas**: 13, 13, 16, 17
* **Engrenages du haut**: 19, 19, 49, 71

**R obtenu** = 19x19x49x71 / (13x13x16x17) = 27.311593282, erreur de 0.00004% 
{: .text-justify}

>Voilà j'ai un beau petit programme écrit en **Python** qui me trouve de belles solutions en quelques secondes ou minutes sur Raspberry pi4 (4Go mini). Il va me permettre de configurer tous les engrenages de mon [#planétaire](https://papsdroidfr.github.io/tags/#planétaire) mécanique.
{: .text-justify}
