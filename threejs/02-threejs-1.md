# Déplacements

Temps de développement : 2 semaines

---

Dans une scène en réalité virtuelle, il est probable que l'ai besoin de se déplacer. Par défaut, il est possible de se déplacer dans la scène 3D en même temps que l'on se déplace "dans le monde réel". Cependant, il peut être utile de pouvoir se déplacer en VR tout en restant statique dans le monde réel, pour cela il est nécessaire de configurer des commandes de déplacement sur les contrôleur. 

Pour être le plus libre possible dans les déplacements, nous avons implémenté des déplacements sur l'axe horizontal et vertical ainsi que des rotations. 

Pour pouvoir expérimenter et définir des mouvements les plus naturels possibles, nous avons créé des variantes de chaque mouvement : 
- La rotation peut se faire de façon continue ou de façon discrète avec des rotations par palier de 30 degrés. 
- Le déplacement vertical peut être toujours activé ou non et peut être contrôlé aussi bien avec les boutons A et B qu'avec un joystick. 

Voici une carte des méthode de déplacement plus détaillée : 

```{figure} ../images/controls.png
:alt: Images des controles à la manette
:align: center

Controles à la manette
```

## Comment fonctionne un déplacement en WEBXR ?

Dans WEBXR, un déplacement dans la scène se fait en modifiant l'espace de référence du joueur et non pas en modifiant la position du joueur. 

Il y a différents jeu de coordonnées à prendre en compte pour comprendre cette modification : Le repère de coordonnées physique (lié aux capteurs et au dispositif de suivi) reste toujours fixe dans l'espace réel. C'est un système de coordonnées immuable qui se base sur la position initiale du casque au lancement du monde. L'espace de référence virtuel (que nous manipulons en WebXR) est un système de coordonnées qui définit comment le contenu virtuel est positionné par rapport au monde physique.

C'est l'espace de référence virtuel qui est transformé par rapport à ce repère physique. Le joueur reste fixe dans le repère physique (il ne bouge pas réellement), mais puisque nous transformons l'espace de référence virtuel, tous les objets virtuels semblent se déplacer autour de lui, ce qui crée une illusion de déplacement.

```{figure} ../images/referencespace.png
:alt: Illustration d'un déplacement de l'espace de référence
:align: center

Modification de ReferenceSpace
```

Par exemple, voici la fonction de déplacement (simplifiée) : 

```javascript
movePlayer() {
    
    const referenceSpace = renderer.xr.getReferenceSpace();
    
    const moveX = this._leftHandController.thumbStick.x;
    const moveZ = this._leftHandController.thumbStick.y;
    
    const speed = 0.03; //meter or unite by frame
    
    const forward = new THREE.Vector3();
    forward.copy(this._head.forward);
    forward.y = 0;
    forward.normalize();
    
    const right = new THREE.Vector3();
    right.copy(this._head.right);
    right.y = 0;
    right.normalize();
    
    const moveDirection = new THREE.Vector3();
    moveDirection.addScaledVector(forward, -moveZ);
    moveDirection.addScaledVector(right, moveX);
    

    /* --- Update of ReferenceSpace --- */

    const offsetTransform = new XRRigidTransform(
        {
            x: - moveDirection.x * speed,
            y: 0,
            z: - moveDirection.z * speed
        },
        {x: 0, y: 0, z: 0, w: 1}
    );
    
    const newReferenceSpace = referenceSpace.getOffsetReferenceSpace(offsetTransform);
    renderer.xr.setReferenceSpace(newReferenceSpace);
}
```
On récupère les inputs provenants des joysticks et la direction dans laquelle le joueur regarde, ensuite on crée un nouveau vecteur de direction que l'on utilise pour créer un décalage sur l'espace de référence.

## Cas de la rotation

Pour créer les mouvements de rotation, la modification de l'espace de référence se passe en 3 parties : 
- Déplace le joueur à l'origine de l'espace
- On applique la rotation 
- On replace le joueur à sa position initiale

Sans revenir au centre de l'espace pour appliquer la rotation, le joueur tournerait autour du centre de l'espace de référénce au lieu de tourner sur lui-même. 

Second point à prendre en compte : lorsque que l'on effectue d'une rotation en réalité virtuelle, il est possible d'être sujet à des maux de coeurs, pour attenuer ce phénomène on peut utiliser des mouvements discrets ou créer une accélération et une décélération au début et à la fin du mouvement lorsqu'il est continue. 

# Bilan

L'implémentation des contrôles est plus longue qu'avec Unity, du fait qu'il faille tout développer à la main. Cepedant, il existe probablement des librairies permettant d'accélérer le developpement des contrôles. 

En ce qui concerne la récupération des inputs provenant des controlleurs, l'accès aux éléments de base comme les boutons ou joysticks est assez facile. 

*Ajouter un bilan sur les méthodes de déplacements*

