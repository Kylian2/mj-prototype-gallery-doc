# Les collisions

Temps de développement : 2~3 jours

--- 
Dans le contexte d'un jeu de jonglage, la volonté de prendre des balles risque d’apparaître assez rapidement. Pour savoir quand prendre les balles, nous allons avoir besoin d’être capables de détecter des collisions, notamment entre le contrôleur du joueur et un objet 3D. Ainsi, en détectant les collisions, nous allons pouvoir savoir si le joueur entre en contact avec l’objet, reste en contact ou quitte l’objet, afin d’associer des actions à ces événements.

Comme dit plus tôt, nous avons implémenté 3 actions : 
- `onColliderEnter`: Le moment où une collision est détectée entre un objet et une des cibles pouvant entrer en collision avec lui. 
- `onColliderStay`: Quand deux objets restent en collision. 
- `onColliderExit`: Le moment où la colision s'arrête.

## Comment est calculée une collision ?

Puisque nous sommes dans le cadre d’un jeu de jonglage, nous allons nous concentrer sur les collisions entre sphères (qui sont d’ailleurs les plus faciles à mettre en place). Une sphère est donc composée d’un centre et d’un rayon. Ainsi, la distance entre le centre et son extrémité est égale au rayon. Deux sphères sont en collision si la distance entre leurs deux centres est inférieure à la somme de leurs deux rayons.

```{figure} ../images/collider_cercle.png
:alt: Illustration des collisions entre sphères
:align: center
:height: 300px

Collisions entre sphères
```

*Remarque : si une cible ou un objet à un rayon de 0 (ou très proche de 0), il est considéré en collision si son centre entre dans l'objet*

## Le Raycasting

Le raycasting est utilisé pour savoir ce que les controlleurs du joueurs pointent, c'est utile pour pouvoir intéragir avec des éléments à distance. ThreeJS mets à disposition des éléments permettant d'intégrer facilement ce mécanisme. La methode `raycaster.intersectObjects(targets: Array, recursive: Boolean);` (avec `targets` une liste de cible à tester, et comme second paramètre un boolean indiquant si la recherche d'intersection est récursive) renvoie une liste contenant chacun des objets sur la trajectoire du rayon.

## Les difficultés rencontrés 

## Bilan

Le système de collision est à nouveau une fonction qu'il faut redévelopper en ThreeJS et qui est disponible nativement dans Unity, cela coute du temps. Si les collisions ont besoins de devenir plus complexe que de la detection à base de rayon, il faut prévoir du temps supplémentaire pour developper d'autres mécanismes (avec un cube par exemple) qui semblent plus compliqués à mettre en place. 