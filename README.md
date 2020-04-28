# template for roll20 pathfinder
Proposition d'une fiche de personnage pour le jeux Pathfinder.
Le but est de coller au mieux aux règles tout en simplifiant les calculs rébarbartifs, néanmoins le joueur est mis à contribution pour les parties importantes, pour éviter de trop le guider, qui pourrait se reposer sur la fiche sans comprendre les règles.

La mise en page se veut "responsive friendly"; à comprendre que la fiche de personne peut s'utiliser dans différentes tailles de fenêtre sans altérer la mise en page.
La mise en page CSS utilise une structure pouvant être réutilisé sans modification du code CSS, voir les [règles](README.md#règles-css) pour d'amples informations.

## Notes pour translation.json
- la propriété ***language*** est necessaire pour une fonction de tri des chaînes de caractères.
  Elle doit correspondre au codage en deux caractères de la norme ISO 639-1
- remplacer le fichier ***translation.json*** par celui de la langue choisie dans le répertoire language

## Fonctionnalitées
la fiche est basé sur pathfinder 2nd édition, les sources vienent du site [d20pfsrd](https://www.d20pfsrd.com) et [pathfinder-fr](https://www.pathfinder-fr.org)

les fonctionnalitées sont les suivantes :
- template pour un personnage et un animal/familier/compagnon animal
- les lancés de combat peuvent se faire par ciblage ou non ( selon les options )
- envoie des jet dans le chat ou uniquement au maître du jeu ( selon les options )
- utilisation de la fiche avec l'unité souhaitée ( longueur, distance et poids )
- commentaire complet au survol de la souris pour chaque lancé de dés
- application des status par un click
- gestion des malus lié à l'âge du personnage
- classes, langues et races préintégrés dans la fiche
- calcul automatique des tailles et poids des personnages et déplacement selon la race et ses caracétristiques
- gestion des malus de charge selon le poid des objets
- pour le sorts gestion des échecs aux sorts profanes lors d'un lancé de dé, et prise en compte de la résistance magique
- glissé déposé du compendium Pathfinder pour les objets et les sorts ( avec conversion des unités et selon la catégorie de la taille si souhaité par l'utilisateur )
- création d'une attaque a partir d'une arme de l'inventaire via un bouton
- transfert des armes et objets dans un dépôt ( utile pour la gestion des charges transportables )

## Modifications des règles Pathfinder
certains point sont discutables pour l'interprétation de certaines règles, voici la liste de ce qui a été décidé :

- les status concernant les CA ne sont pas toujours clair dans les règles qaund la CMD est concernée ou pas, dans le doute sans mention contraire le status est appliqué à la CA et la DMD.
- le poids transportable utilise un tableau pour déterminer ses valeurs, par simplicité la formule suivante a été retenu 
  > Force compris entre 0 à 10 : 5 x Force
  
  > Force supérieuré à 10 : 24.9087 e^( 0.1386 x Force )
 
- L'âge limite pour la jeunesse n'est pas définit clairement dans les règles, donc les personnages auront les malus de la jeunesse tant qu'ils n'ont pas atteient l'âge adulte.

## Modification de la fiche prévues

- Global
  - ajouter fiche PNJ ( utiliser variables disctinctes commencant par "monster_") se baser sur fiche PNJ
  - ajout des variables d'initialisation dans sheet.json [Default Sheet Settings](https://wiki.roll20.net/Default_Sheet_Settings) [example](https://github.com/MadCoder253/roll20-character-sheets/blob/master/GURPS/sheet.json)
    
- Entête
  - ajouter l'ajout de status personnalisé ( un titre, le label a affiché dans status,un bouton d'activation + panneau déroulant contenant un repeating pour ajouter les attributs à modifer
  )
    
- Personnage
  - voir pour implémentation des niveau négatifs pour la vie alternative [blessure vitalité](https://www.pathfinder-fr.org/Wiki/Pathfinder-RPG.Blessures%20et%20vitalit%C3%A9.ashx)

- Spécialisation

- Combat

- Magie

- Défense

- Compétences

- Inventaire

- rolltemplate
  
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

- général
  - liaison champ avec un token, uniquement les input simples ( sans **disable** ou valeurs calculées ) peuvent être lié à un token, la seule solution est d'utiliser du ECMAscript pour changer la valeur
- sheet.json
  - pour utiliser la traduction de **description**, utiliser **displaytranslationkey** ( **description** peut etre omis )
  - pour utiliser la traduction de **description**, utiliser **descriptiontranslationkey** (**description** doit avoir une valeur autre que null ou vide, utiliser par exemple **"foo"** )
- HTML
  - **ne pas** utiliser le terme **_max** ou **_maximum** pour les variables utilisés dans des calculs; des comportements aléatoires sont à prévoir ( par exemple "@{hitpoints_max}" renvoie toujours 0 ), les réservé pour les input à lié avec les tokens. **Attention** à mettre la valeur entre paranthèses si elle contient plusieurs variables, par exemple : ( @{var1}+@{var2} )
  - pour un "radio" les inputs **doivent** se suivre dans le code et **doivent** avoir l'attribute value
  - les balises html5 dans leur majorité ne sont pas autorisés
  - les attributes "data" pour les balises sont supprimés
  - pour les fieldset "repeating_xxx" ne pas utliser les undescore pour le nommage de la classe, mais faisable pour les variables dans le repeating, par exemple "repeating_var1_var2_var3" -> "repeating_var1" + "var2_var3"
    
- ECMAscript
  - "<script data-type="text/worker>" est valide pour roll20 ( au lieu de <script type="text/worker"> ), utile pour un interpréteur ECMAscript pendant le dev.
  - utiliser removeRepeatingRow() ne déclenche pas les events **on("change:foo")** ni **on("remove:foo")**, compensable si suivit de **setAttrs(payload, {silent:true}, callback)** ou **setAttrs(payload, {silent:false}, callback)**
    
- CSS
  - les règles pour "rolltemplate" sont indépendants du "character sheet"
  - les input ont la règle "width" trop restrictif; obligation d'utiliser "important" pour appliquer un style personnalisé
  - les règles sur "html" sont ignorées, donc au revoir les tailles en "rem"
  - les images en base64 ne peuvent être intégrés dans les styles CSS
- SheetWorker
  - roll20 ne gère pas les négatifs de négatifs, pour gérer les négatifs on doit utiliser ***-(@{variable})***
  - les champs sont pensés **uniquement** pour les nombres, (disabled="disabled", type="hidden", value=@{[...]}, active ces fonctions ).
    
  pour travailler sur des string il est **obligatoire** d'utiliser un type "text" et l'attribut "readonly"
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
        - x < y :
          > {{#rollLess() x y }} ... {{/rollLess() x y }}
        - x <= y :
          > {{#^rollGreater() x y }} ... {{/^rollGreater() x y }}
        - x = y :
          > {{#rollBetween() x y y }} ... {{/rollBetween() x y y }} 
        - x > y :
          > {{#rollGreater() x y }} ... {{/rollGreater() x y }}
        - x >= y :
          > {{#^rollLess() x y }} ... {{/^rollLess() x y }}
