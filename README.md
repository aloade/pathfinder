# template for roll20 pathfinder
Proposition d'une fiche de personnage pour le jeux Pathfinder.
Le but est de coller au mieux aux règles tout en simplifiant les calculs rébarbartifs, néanmoins le joueur est mis à contribution pour les parties importantes, pour éviter de trop guider le joueur qui pourrait se reposer sur la fiche sans comprendre les règles.

La mise en page se veut "responsive friendly"; à comprendre que la fiche de personne peut s'utiliser dans différentes tailles de fenêtre sans altérer la mise en page.
La seule exeption étant le menu qui a une obligation d'écriture imposée par roll20, empêchant sont utilisation selon les standards HTML.
La mise en page CSS utilise une structure pouvant être réutilisé sans modification du code CSS, voir les règles plus bas pour d'amples informations.

## Modifications prévues
- Global
    - ajouter les langues.
    - remplacer la fonte "formal436" par une fonte sans ayant-droit.
    - ajouter mise en page pour PJ/PNJ.
    
- Entête
    - supprimer le champ "Vitalité" compliquant les rêgles sur la vie.
    - remplacer le terme "Santé" par "Points de vie".
    
- Personnage
    - changer "Stat." par "Carac.", refléter le "title" en fonction.
    - changer le "title" pour tous les lancer de dés pour remplacer "1d20 + Total" par "1d20 + Mod.".
    
- Combat ( attaques )
    - définir minimum des dégâts à 1, utiliser :
      > /roll { 1d1, 1d20+@{attribut} }d1
    - changer le modificateur de caractéristiques des dégâts; ajouter la "Dextérité", refléter le "title" en fonction.
    - jet d'attaque changer le "title" du modificateur de caractéristique pour préciser les cas d'utilisation par défaut de "Force" (CaC) ou "Dextérité" (Distance).
    - revoir le calcul des critiques, lancé de jet d'attaque :
        1. ( jet d'attaquen n°1 < CA adverse ) -> pas de de dégâts
        2. ( CA adverse <= jet d'attaque n°1 < 20 ) -> dégâts normaux
        3. ( jet d'attaque n°1 = 20 ) -> nouveau lancé de jet d'attaque :
            1. ( jet d'attaque n°2 < CA adverse ) -> dégats normaux.
            2. ( jet d'attaque n°2 >= CA adverse ) -> dégats critiques

      voir [pathfinder wiki](https://www.pathfinder-fr.org/Wiki/Pathfinder-RPG.Valeurs%20de%20combat.ashx).

    - ajouter un champ "Bonus Confirmation aux Critiques" ( s'additionne au "jet d'attaque n°2" cité précédement ).
      
    - ajouter un champ "Bonus Confirmation aux Critiques" à la CA ( s'additionne à la CA ).
    
- Combat ( Manoeuvres de combats )
    - relier les jets de BMO au DMD adverse (comme une attaque), ajouter dans le "rolltemplate" la différence des deux résultats (utile pour le MJ).
    - déplacer le DMD dans "Défense", ajouter une zone de commentaire, renommer "Manoeuvres de combats" par "Bonus manoeuvre offensive".
    
- Magie ( Sortilèges )
    - déplacer la navigation des sorts par une barre verticale sur la gauche du contenu
    - ajouter une case numérique "Divers" pour le degré de difficulté.
    
- Magie ( Attaque de contact )
    - changer le modificateur de caractéristiques pour afficher "Force" et "Dextérité", refléter le "title" en fonction.
    
- Compétences
    - limiter les points de "rangs" au niveau du personnage
    - remplacer les calculs :
      > ((((x * @{attribut}) + 3) - abs((x * @{attribut}) - x)) / 2)
      
      par
      > 3 * ( @{attribut} + 1 - abs( @{attribut} - 1 ) ) / 2
      
- Inventaire
    - ajouter un bouton pour masquer le poids des armes, armures, et objets ( masquage via CSS ? )
    - ajouter la mécanique de surcharge, voir sur [pathfinder fr](https://www.pathfinder-fr.org/Wiki/Pathfinder-RPG.Poids%20transportable.ashx).
    
- Inventaire ( Armes )
    - pour la colonne "Type", ajouter un "title" pour expliquer les termes "T", "C", "P"
    - ajouter un bouton ( voir sur [roll20.net](https://wiki.roll20.net/Sheet_Worker_Scripts) section "clicked:<button_name>") pour ajouter un champ dans "repeating" de "Attaques" ( voir [roll20.net](https://wiki.roll20.net/Sheet_Worker_Scripts) section "generateRowID()"; pensez à vérifier que ca correspond pas un ID déjà créé ).

## Règles CSS
Compilation des règles CSS utilisable pour la mise en page.

- disposition flex
  flex-row
  flex-col flex-col2 flex-col3 ... flex-col11
  flex-col-auto
  
- input-group
    input-group
    input-group-prepend input-group-append input-group-text

- accordéon
    accordion
    accordion-checkbox accordion-label
    accordion-container
    
- switch
    switch
    switch-on switch-off

- flip coin
    coin coin-content
    coin-on coin-off

- style générique
    center
    strong
    bold
    alert
    
- input
    fixed-small fixed-medium fixed-large

    
## Remarques sur roll20 et la création de la fiche de personnage
quelques "pense-bête" pour certains aspects pas évident à deviner lors de la création de la fiche de personnage.

- HTML
    - pour un "radio" les input DOIVENT se suivre dans le code et DOIVENT avoir l'attribute value
    - les balises html5 dans leur majorité ne sont pas autorisés
    - les attributes "data" pour les balises ne sont pas autorisées
    - pour les fieldset "repeating_xxx" ne pas utliser les undescore pour le nommage de la classe
    
- CSS
    - les règles pour les rolltemplate sont indépendant du "character sheet"
    - les input ont la règle "width" trop restrictif; obligation d'utiliser "important" pour appliquer un style personnalisé
    
- SheetWorker
    - si des repeating sont en cause, les résultats des calculs doivent être envoyé vers des input "hidden"
      ( quand l'attribut "disabled" est présent les calculs sont 'parasités' )
    - getAttr renvoie l'attribut "value" brut
      ( la valeur n'est pas calculé à la volée et renvoi un string brut )
    - si un input avec l'attribut "disabled" a un calcul incluant un négatif d'un négatif, le résultat échoue silencieusement ?!
    
- champ autocalc
    - pour afficher une valeur à zéro ou un nombre donné, avec une entrée à 0 ou 1 ( checkbox de roll20 par exemple ), utiliser le calcul suivant :
      > x * ( @{attribut} + 1 - abs( @{ attribut } - 1 ) ) / 2
      
      où "x" est la valeur souhaitée si non zéro.
    
- rollTemplate
    - pas de calculs conditionnels utilisable, uniquement de l'affichage
      par exemple pour s'assurer qu'une valeur est au minimum à 1, utiliser :
      > /roll { 1d1, 1d20+@{attribut} }d1
      
      ou pour avoir une valeur maximale à 20, utiliser :
      > /roll { 1d0+20, 1d20+@{attribut} }k1 
