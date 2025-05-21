# Jongler en réalité virtuelle

Temps de developpement : 3/4 jours (incluant la découverte de unity).

Version de Unity : 6.0 LTS.

Utilisation de Open XR avec le XR Interaction Toolkit (installation supplémentaire requise).
## Scénario de l'application

L'utilsateur peut choisir des balles posées sur la table, il peut jongler avec les balles en faisant des lancers de 1, 2 ou 3 par détection du mouvement des main (detection selon l'angle du geste) ou faire des lancers de 2 ou 3 avec les touches de manettes. L'utilisateur peut attraper des balles avec une pression sur une touche et faire des lancers libres en relachant.  

Chaque balle possède son propre son, qui est joué à chaque fois qu'elle est réceptionnée (atteint la zone rouge, voir fig. 1).

```{figure} ../images/unity3D-jonglage.png
:label: myFigure
:alt: Aperçus de l'application
:align: center

Aperçus de l'application
```

## Travail Réalisé

Le premier travail a été d’ajouter les contrôles via les manettes, une étape relativement rapide grâce aux composants prédéfinis de Unity 3D.

Ensuite, il a fallu modéliser les balles : ce sont des sphères auxquelles on ajoute des scripts et une classe Ball, comportant les propriétés spécifiques de chaque balle, notamment les trajectoires associées aux mouvements 1, 2 et 3. D’autres composants sont également ajoutés à ces balles afin de gérer les collisions et les effets sonores.

Il est également nécessaire de développer un script à appliquer aux contrôleurs, permettant de détecter le sens des mouvements et leur vitesse, en calculant les variations de position dans le temps.

## Bilan

Les composants et scripts préfait de Unity facilite beaucoup le travail. En revanche une mise à jour de XR Interaction Toolkit rend beaucoup de tutoriels obsolètes. 

La detection des mouvements grâce à la position n'est pas toujours très précise ou réactive. L'idée de méler de l'apprentissage pour s'adapter aux mouvements propre à chacun a été évoquée.