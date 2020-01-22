# template for roll20 pathfinder
Proposition d'une fiche de personnage pour le jeux Pathfinder.
Le but est de coller au mieux aux règles tout en simplifiant les calculs rébarbartifs, néanmoins le joueur est mis à contribution pour les parties importantes, pour éviter de trop le guider, qui pourrait se reposer sur la fiche sans comprendre les règles.

La mise en page se veut "responsive friendly"; à comprendre que la fiche de personne peut s'utiliser dans différentes tailles de fenêtre sans altérer la mise en page.
La mise en page CSS utilise une structure pouvant être réutilisé sans modification du code CSS, voir les [règles](README.md#règles-css) pour d'amples informations.

## Modifications prévues
- Global
    - remplacer la fonte "formal436" par une fonte sans ayant-droit.
    - ajouter mise en page pour PJ/PNJ.
    - supprimer le texte pour les "data-i18n" (h1, span, label, placeholder, option, title )
    
- Entête ( status )
    - prendre en charge la perte de bonus de caractéristique ( -> base = 10 & mod = 0 )
    - prendre en charge caractéristique vaut 0 ( -> base = 0 & mod = -5 )
    
- Personnage

- Combat ( initiative )
    - ajouter un bouton pour passer son tour ( commande **!eot** ? )
    
- Combat ( attaques )
    - définir minimum des dégâts à 1, utiliser :
      > /roll { 1d1, { 1d20+@{attribut} } }dl1
    - changer le modificateur de caractéristiques des dégâts; ajouter la ***Dextérité***, refléter le ***title*** en fonction.
    - jet d'attaque changer le ***title*** du modificateur de caractéristique pour préciser les cas d'utilisation par défaut de ***Force*** (CaC) ou ***Dextérité*** (Distance).
    - ajouter un champ ***Bonus confirmation critique***
    - revoir le jet d'attaque et de dégâts :
        1. ( ***jet d'attaque  n°1*** < ***CA adverse*** ) -> pas de de dégâts
        2. ( ***CA adverse*** <= ***jet d'attaque n°1*** < ***critique de l'arme*** ) -> dégâts normaux + dégâts sup.
        3. ( ***jet d'attaque n°1*** >= ***critique de l'arme*** ) -> nouveau lancé de jet d'attaque :
            1. ( ***jet d'attaque n°2*** + ***Bonus confirmation critique*** < ***CA adverse*** ) -> dégats normaux + dégâts sup.
            2. ( ***jet d'attaque n°2*** + ***Bonus confirmation critique*** >= ***CA adverse*** ) -> dégats critiques + dégâts sup.
            
      afficher la valeur du jet de ***confirmation de critique*** dans le rollTemplate.
    - ajouter champ "dégâts supplémentaires" ( précis, sournois, élémentaire, ... )
      
      voir [pathfinder wiki](https://www.pathfinder-fr.org/Wiki/Pathfinder-RPG.Valeurs%20de%20combat.ashx).
    - ajouter un champ ***Bonus Confirmation aux Critiques*** ( s'additionne au ***jet d'attaque n°2*** cité précédement ).
    
- Combat ( Manoeuvres de combats )
    - relier les jets de ***BMO*** au ***DMD*** adverse (comme une attaque), ajouter dans le "rolltemplate" la différence des deux résultats (utile pour le MJ).

- Magie ( Sortilèges )
    - ajouter un encart pour afficher la valeur de ***échecs aux sorts profanes***
    - pour les sorts, ajouter un bouton pour le lancer; affiche le résultat dans un "rollTemplate" dédié :
      - le ***NLS*** vs ***RM*** de la cible, si ca touche afficher les dégâts et effet (description du sort ? ), ainsi que le ***DD*** du sort et le jet eventuel que la cible doit faire.
      - Prendre en compte les ***échecs aux sorts profanes*** de l'armure (champ déroulant utilisé/pas utilisé) ainsi que la composante gestuelle
      - ***rollFailureSpell*** = "/roll 1d100"
      - ***FailureSpellTotal*** = ***FailureSpell*** x ***FailureSpellUsed***( 0 ou 1 ) x ***composante gestuelle***( 0 ou 1 )
      ```
      ... afficher les informations du sorts ...
      {{#<FailureSpellTotal>}}
      ... afficher les info du test aux échecs de sorts profanes ...
      {{/<FailureSpellTotal>}}
      {{#rollLess() rollFailureSpell FailureSpellTotal}}
      ... afficher la réussite du sort ...
      {{/rollLess() rollFailureSpell FailureSpellTotal}}
      
      {{#^rollLess() rollFailureSpell FailureSpellTotal}}
      ... afficher l'échec du sort ...
      {{/^rollLess() rollFailureSpell> FailureSpellTotal}}
      ```

- Défense

- Compétences
    - limiter les points de ***rangs*** au niveau du personnage
      
- Inventaire ( Armes )

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
    - pour un "radio" les input **doivent** se suivre dans le code et **doivent** avoir l'attribute value
    - les balises html5 dans leur majorité ne sont pas autorisés
    - les attributes "data" pour les balises sont supprimés
    - pour les fieldset "repeating_xxx" ne pas utliser les undescore pour le nommage de la classe
- CSS
    - les règles pour "rolltemplate" sont indépendant du "character sheet"
    - les input ont la règle "width" trop restrictif; obligation d'utiliser "important" pour appliquer un style personnalisé
    - les règles sur "html" sont ignorées, donc au revoir les tailles en "rem"
    - les images en base64 ne peuvent être intégrés dans les styles CSS
- SheetWorker
    - les champs sont pensés **uniquement** pour les nombres, (disabled="disabled", type="hidden", value=@{[...]}, active ces fonctions ).
    
    pour travailler sur des string il est **obligatoire** d'utiliser "<input type="text" name="[...]" value="[...]" /> (voir du côté de "readonly" )
    - si des repeating sont en cause, les résultats des calculs doivent être envoyés vers des input "hidden"
      ( quand l'attribut "disabled" est présent les calculs sont 'parasités' )
    - getAttr renvoie l'attribut "value" brut
      ( la valeur n'est pas calculé à la volée et renvoi un string brut )
    - si un input avec l'attribut "disabled" a un calcul incluant un négatif d'un négatif, le résultat échoue silencieusement ?!
    - les bouton de type "action" ne doivent pas contenir d'underscore.
- champ autocalc
    - pour afficher une valeur à zéro ou un nombre donné, avec une entrée à 0 ou 1 ( checkbox de roll20 par exemple ), utiliser le calcul suivant :
      > x * ( @{attribut} + 1 - abs( @{ attribut } - 1 ) ) / 2
      
      où "x" est la valeur souhaitée si non zéro.
      
- translation.json
    - le message d'erreur "Foudn a pre-defined key order!" correspond à une liste d'élément ordonné contenant une erreur.
- rollTemplate
    - pas de calculs conditionnels utilisable, uniquement de l'affichage
      par exemple pour s'assurer qu'une valeur est au minimum à 1, utiliser :
      > /roll { 1d1, { 1d20+@{attribut} } }dl1
      
      ou pour avoir une valeur maximale à 20, utiliser :
      > /roll { 1d0+20, { 1d20+@{attribut} } }kh1 
    - règles pour les inégalités
        - x = y :
          > {{#rollBetween() x y y }} ... {{/rollBetween() x y y }} 
        - x < y :
          > {{#rollLess() x y }} ... {{/rollLess() x y }}
        - x <= y :
          > {{#^rollGreater() x y }} ... {{/^rollGreater() x y }}
        - x > y :
          > {{#rollGreater() x y }} ... {{/rollGreater() x y }}
        - x >= y :
          > {{#^rollLess() x y }} ... {{/^rollLess() x y }}
