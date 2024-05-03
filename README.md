# Intrusion en pays zombie

## Mise en place

### api

```sh
composer install
php -S localhost:8000 api/index.php
```

### front
```sh
npm install
npm run dev
```

## Votre mission

* Que fait cette application ?


        C'est un jeu dans lequel le joueur doit naviguer à travers un pays infesté de zombies et de "HACKS" (Humains Armés de Concombres, Kiwis et Salsifis). Le personnage principal, DÉDÉ, possède la capacité de se téléporter sur une case du pays des zombies et d'indiquer le nombre de HACKS présents dans les cases qui sont autour. Le but du jeu c'est d'éviter les HACKS tout en aidant les zombies à survivre. Si DÉDÉ atterrit sur une case déjà occupée par un HACK, le joueur perd la partie.

* Comment est-elle architecturée ? Quels sont les avantages et les inconvénients de cette architecture ?

          L'architecture de cette application est une API REST. 
          
          Il y a beaucoup d'avantages, par exemple la simplicitée. 
          Les API REST suivent des principes simples, comme les requêtes HTTP et les actions CRUD (Créer, Lire, Mettre à jour, Supprimer). 
          Cela les rend faciles à comprendre et à utiliser pour les développeurs.
          
          Et bien sur il y a des inconvénient par exemple il manque de standardisation bien que REST API ait des principes de base, mais il n'y a pas de standard unique pour la mise en oeuvre des API REST..... 

* Que doit-on modifier le nombre de lignes et de colonnes ?

         je ne suis pas sur d'avoir bien compris dans quel endroit, si c'est dans le ficher `Game.php` on doit ajuster dans la class Game les valeurs des propriétés $nbcols (nombre de colonnes) et $nbrows (nombre de lignes). 
         
* Quel est l'autre paramètre à cet endorit ? que représente-t-il ?

         un autre paramètre qui est ecrit dans le code c'est $hackRatio, qui est utilisé dans la méthode init() pour déterminer le nombre initial de cellules "HACK" dans le jeu. il détermine la probabilité qu'une cellule soit "HACK" lors de l'initialisation du jeu.

* Dans le cas où $game->cells représente la configuration suivante :

|       | X     |       |       |       |       |       |
|-------|-------|-------|-------|-------|-------|-------|
|       |       | **X** | **X** |       | **X** |       |
|       | **X** |       | **X** |       |       | **X** |
|       |       |       |       | **X** |       |       |
| **X** |       |       |       |       |       |       |

Que renverra $game->getNeighbours("2.5") ?

        la méthode getNeighbours() 

       Elle prend en entrée l'identifiant d'une cellule au format "ligne.colonne".

        Elle retourne un tableau contenant les identifiants des cellules voisines de la cellule donnée.

        du coup $game->getNeighbours("2.5") va retourner les identifiants des cellules voisines de la cellule "2.5" dans la grille si j'ai bein compris.... :/ 
        
        donc le $game->getNeighbours("2.5") va envoyer 

        (1,4)Pas de HACK ?  ,  (1,5)Pas de HACK ?  , (1,6)Pas de HACK ?
        (2,4)Pas de HACK ?  ,  (2,5) DÉDÉ         , (2,6)Pas de HACK  ?
        (3,4)Pas de HACK ?  ,  (3,5)Pas de HACK ?  , (3,6)Pas de HACK ?
        

* Quelle "critique" pourrait-on faire sur l'architecture fonctionnelle ?
    * _Indice :_ Tricheur

                Manque de Séparation des Responsabilités car le fichier Game.php contien à la fois la logique métier du jeu, et la logique de gestion des données...  normalment, ca devraient être séparées pour améliorer la lisibilité et la maintenabilité du code.

* Comment pourrait-on y pallier ?
    * _Indice :_ il y a au moins deux solutions : une solution simple à mettre en oeuvre avec les outils en place,
      et une solution nécessitant l'ajout d'une autre brique dans l'architecture technique)

            Premiere solution: 

            Créez des services dédiés pour chaque aspect d'application, par exemple pour la gestion du jeu  
            Chaque service ne devrait être responsable que d'une seule tâche.
            (je ne suis pas sur que c'est plus simple... xD) 

            Douzième Solution:
            Adopter une architecture basée sur le modèle MVC (Modèle-Vue-Contrôleur) où les contrôleurs se concentrent uniquement sur la gestion des requêtes et des réponses HTTP, et la logique métier est déplacée vers des services d'application distincts par exemple dans le dossier src/Entity (Services d'Entités).
        
                

* Quelles améliorations peut-on apporter à la structuration du front ? de l'API ?            
* Autrement dit, que pourrait-on changer qui rende le code plus structuré, sans différence pour l'utilisateur ?

        API: Adopte une architecture basée sur le modèle MVC (Modèle-Vue-Contrôleur) où les contrôleurs se concentrent uniquement sur la gestion des requêtes et des réponses HTTP, et la logique métier est déplacée vers des services d'application distincts par exemple dans le dossier src/Entity (Services d'Entités).
         
          En déplaçant la logique métier hors des contrôleurs et en la plaçant dans des services d'application distincts, je vais adopter une approche de conception plus modulaire et flexible.

        Front:
        (exemple le fichier Grid.vue)

        Utilisation de CSS Modulaire:
        Pour améliorer la lisibilité du code, je déplacerais tous les styles CSS actuellement inclus dans le fichier grid.vue vers un fichier CSS dédié. Cela rendrait le code plus clair et plus facile à comprendre

        Déplacer la Logique de Données : La logique pour initialiser la grille et interagir avec l'API pourrait être déplacée dans des méthodes séparées, plutôt que d'être directement intégrée dans le <script setup>. 
        Cela aiderait à mieux organiser le code en le séparant en parties distinctes, ce qui le rendrait plus facile à comprendre et à garder à jour.

        Toutes ces modifications rendront le code plus structuré, sans différence pour l'utilisateur.

* Quelles critiques pourrait-on formuler au développeur sur sa manière d'utiliser git sur ce projet ?

          Le développeur travaillait directement sur la branche principale, ce qui est une erreur. En fait, on devrait créer une branche de développement (dev) et tirer les branches avec les futures modifications à partir de cette branche. Par exemple, avec Git, on peut créer une nouvelle branche de développement en utilisant la commande git checkout -b dev, puis une branche pour une fonctionnalité spécifique, par exemple git checkout -b Feature_crud. Une fois le développement terminé et que l'on est sûr que tout fonctionne correctement, on fusionne la branche de la fonctionnalité avec la branche de développement (dev) et on supprime ensuite la branche de la fonctionnalité.
          
           En utilisant cette méthode avec Git, on s'assure qu'il n'y aura pas de problèmes avec la gestion de l'application. 
           De plus, si une erreur est commise lors d'un commit, il est toujours possible de revenir en arrière.

* _Indice réponse niveau 1 :_ git log
* _Indice réponse niveau 2 :_ git reflog
