# Unity 3D

Pour le developpement sous Unity 3D, nous nous sommes appuyé sur l'interface de programmation OpenXR pour assurer, malgré notre développment sur Meta Quest 3, une compatibilité la plus large possible avec les autres casques du marché. 

### Qu'est-ce que Unity ?

Unity est un moteur de jeu multiplateforme développé par Unity Technologies. Il permet de créer des applications en 2D, 3D, réalité virtuelle (VR) et réalité augmentée (AR).

Ses caractéristiques principales sont : 
- Interface developpement visuelle : Éditeur avec scène, assets, hiérarchie, etc.
- Scripting en C#
- Déploiement multiplateforme : Compatible avec Windows, macOS, Android, iOS, WebGL, consoles, etc.

Le points fort de Unity 3D est que certains composants sont déjà developpés ce qui permet d'économiser du temps de developpement. Par exemple les gestions de déplacements et la récupérations des entrées (via les boutons) sont déjà disponibles.

### Qu'est-ce que OpenXR ?
OpenXR est une API standard ouverte développée par le Khronos Group qui vise à unifier le développement des applications en réalité virtuelle, réalité augmentée et réalité mixte, en permettant d'écrire un seul code compatible avec plusieurs casques.

## Objectifs 

- Estimer la difficulté et le temps de developpement
- Expérimenter la récupération des données des controlleurs (vitesse, accélération)
- Expérimenter l'exportation en WebGL
- Expérimenter la détection de mouvement dans le cadre de la jonglerie pour associer des actions et des lancers de balles

## Critères d'évaluations