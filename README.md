# template for roll20 pathfinder
Proposition d'une fiche de personnage pour le jeux Pathfinder.
Le but est de coller au mieux aux règles tout en simplifiant les calculs rébarbartifs, néanmoins le joueur est mis à contribution pour les parties importantes, pour éviter de trop guider le joueur qui pourrait se reposer sur la fiche sans comprendre les règles.

## modification prévues pour la prochaine version
- En tête
    - supprimer le champ "Vitalité" compliquant les rêgles sur la vie.
    - remplacer le terme "Santé" par "Points de vie"
- Personnage
    - changer "Stat." par "Carac.", refléter le "title" en fonction
    - changer le "title" pour tous les lancer de dés pour remplacer "1d20 + Total" par "1d20 + Mod."
- Combat
    - Attaques : changer le modificateur de caractéristiques des dégâts; ajouter la dextéritié, refléter le "title" en fonction .
    - Attaques : jet d'attaque changer le "title" du modificateur de caractéristique pour préciser les cas d'utilisation par défaut de "Force" (CaC) ou "Dextérité" (Distance).
    - Attaques : revoir le calcul des critiques, car des modificateurs peuvent s'appliquer.
    - Manoeuvres de combats : relié les jets de BMO au DMD adverse (comme une attaque), ajouter dans le "rolltemplate" la différence des deux résultats (utile pour le MJ) et déplacer le DMD dans "Défense"
- Magie
    - Sortilèges : Ajouter une case numérique "Divers" pour le degré de difficulté.
    - Attaque de contact : changer le modificateur de caractéristiques pour afficher "Force" et "Dextérité", refléter le "title" en fonction.
- Compétence
    - Limiter les points de "rangs" au niveau du personnage
- Inventaire
    - Voir pour l'ajouter d'un bouton pour masquer le poids des armes, armures, et objets ( masquage via CSS ? )
    - Armes : pour la colonne "Type", ajouter un descriptif des termes "T", "C", "P"
    - Poids : ajouter la mécanique de surcharge (https://www.pathfinder-fr.org/Wiki/Pathfinder-RPG.Poids%20transportable.ashx)
    
## Remarques concernant roll20 et la création de la fiche de personnage
Beaucoup d'éléments bloquant ne sont pas clairement listés sur les pages d'aide de roll20, voici une liste "pense-bête".

- sheet worker
    - si des repeating sont en cause, les résultats des calculs doivent être envoyé vers des input "hidden"
      ( quand l'attribut "disabled" est présent les calculs sont 'parasités' )
    - getAttr renvoie l'attribut "value" brut
      ( la valeur n'est pas calculé à la volée et renvoi un string brut )
    - si un input avec l'attribut "disabled" a un calcul incluant un négatif d'un négatif, le résultat échoue silencieusement ?!
    
- html
    - pour un "radio" les input DOIVENT se suivre dans le code et DOIVENT avoir l'attribute value
    - les balises html5 dans leur majorité ne sont pas autorisés
    - les attributes "data" pour les balises ne sont pas autorisées

- fieldset "repeating_xxx"
    - ne pas utliser les undescore pour le nommage de la classe

- CSS
    - les règles pour les rolltemplate sont indépendant du "character sheet"
    - les input ont la règle "width" trop restrictif; obligation d'utiliser "important" pour appliquer un style personnalisé
