# Les collisions

Temps de développement : 2~3 jours

--- 
Dans le contexte d'un jeu de jonglage, la volonté de prendre des balles risque d’apparaître assez rapidement. Pour savoir quand prendre les balles, nous allons avoir besoin d’être capables de détecter des collisions, notamment entre le contrôleur du joueur et un objet 3D. Ainsi, en détectant les collisions, nous allons pouvoir savoir si le joueur entre en contact avec l’objet, reste en contact ou quitte l’objet, afin d’associer des actions à ces événements.

Comme dit plus tôt, nous avons implémenté 3 actions : 
- `onColliderEnter`: Le moment où une collision est détectée entre un objet et une des cibles pouvant entrer en collision avec lui. 
- `onColliderStay`: Quand deux objets restent en collision. 
- `onColliderExit`: Le moment où la colision s'arrête.

l est également possible de vouloir sélectionner un élément à distance, par exemple pour appuyer sur un bouton. Cela est géré par les fonctions de raycasting de Three.js.

## Comment est calculée une collision ?

Puisque nous sommes dans le cadre d’un jeu de jonglage, nous allons nous concentrer sur les collisions entre sphères (qui sont d’ailleurs les plus faciles à mettre en place). Une sphère est donc composée d’un centre et d’un rayon. Ainsi, la distance entre le centre et son extrémité est égale au rayon. Deux sphères sont en collision si la distance entre leurs deux centres est inférieure à la somme de leurs deux rayons.

```{figure} ../images/collider_cercle.png
:alt: Illustration des collisions entre sphères
:align: center
:height: 300px

Collisions entre sphères
```

*Remarque : si une cible ou un objet à un rayon de 0 (ou très proche de 0), cela revient à détecter une collision si son centre entre dans l'objet*

## Le Raycasting

Le raycasting est utilisé pour savoir ce que les controlleurs du joueurs pointent, c'est utile pour pouvoir intéragir avec des éléments à distance. ThreeJS mets à disposition des éléments permettant d'intégrer facilement ce mécanisme. La methode `raycaster.intersectObjects(targets: Array, recursive: Boolean);` (avec `targets` une liste de cible à tester, et comme second paramètre un boolean indiquant si la recherche d'intersection est récursive) renvoie une liste contenant chacun des objets sur la trajectoire du rayon.

Le raycasting concerne uniquement les controlleurs. 

## Utiliser un detecteur de collision

Nommons D l'object d'où part la detection et C la cible de l'object D. Pour utiliser un detecteur de collision, il faut ajouter une instance de collider sur D. 

```javascript
D.collider = new XrCollider({
    object: this,
    targets: [],
    radius: 0.001,
    debug: false,
    scene: this.context.scene
});
```

Ensuite à l'aide de la méthode `addTarget(targets)`, on peut ajouter C à la liste des cibles du collider de D. 

Actuellement, dans l’implémentation, pour ajouter une cible au collider** d’un **contrôleur, il existe deux possibilités :

1. Passer par la classe `XRInput` : dans ce cas, aucune distinction n’est faite entre les cibles de collision et les cibles de raycasting ; la cible sera ajoutée aux deux listes de cibles.
2. Passer directement par le contrôleur : cette méthode permet d’ajouter la cible soit à la liste des cibles du collider, soit à la liste des cibles du raycasting.

## Les difficultés rencontrées 

La principale difficulté fut la gestion des cibles au niveau des contrôleurs. En effet, puisque les contrôleurs se connectent et se déconnectent, il est possible qu'à la création de la scène 3D, le contrôleur ne soit pas encore connecté. Il faut donc avoir une liste tampon de cibles et essayer régulièrement d'ajouter aux contrôleurs, si tant est qu’ils existent, les cibles n’ayant pas pu être ajoutées initialement.
De même, lorsqu’un contrôleur se déconnecte, il faut pouvoir récupérer ses cibles pour les attribuer au nouveau contrôleur qui se connectera.

Ces problèmes sont gérés par les classes XRInput et XRMecanicalControllerInput, qui possèdent toutes deux des buffers.

## Bilan

Le système de collision est à nouveau une fonction qu'il faut redévelopper en ThreeJS et qui est disponible nativement dans Unity, cela coute du temps. Si les collisions ont besoins de devenir plus complexe que de la detection à base de rayon, il faut prévoir du temps supplémentaire pour developper d'autres mécanismes (avec un cube par exemple) qui semblent plus compliqués à mettre en place. 