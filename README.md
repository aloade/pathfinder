# template for roll20 pathfinder
Proposition d'une fiche de personnage pour le jeux Pathfinder.
Le but est de coller au mieux aux règles tout en simplifiant les calculs rébarbartifs, néanmoins le joueur est mis à contribution pour les parties importantes, pour éviter de trop le guider, qui pourrait se reposer sur la fiche sans comprendre les règles.

La mise en page se veut "responsive friendly"; à comprendre que la fiche de personne peut s'utiliser dans différentes tailles de fenêtre sans altérer la mise en page.
La mise en page CSS utilise une structure pouvant être réutilisé sans modification du code CSS, voir les [règles](README.md#règles-css) pour plus amples informations.

## Notes pour translation.json
- la propriété ***language*** est necessaire pour une fonction de tri des chaînes de caractères.
  Elle doit correspondre au codage en deux caractères de la norme ISO 639-1
- remplacer le fichier ***translation.json*** par celui de la langue choisie dans le répertoire language

## Fonctionnalitées
la fiche est basé sur pathfinder 2nd édition, les sources vienent des sites, par ordre décroissant d'utilisation [d20pfsrd](https://www.d20pfsrd.com) et [pathfinder-fr](https://www.pathfinder-fr.org)

les fonctionnalitées principales sont les suivantes :
- template pour un personnage, un animal/familier/compagnon animal ou un PNJ
- gestion des status dans une fenêtre unique, avec application de leurs effets en un click
- gestion des malus lié à l'âge du personnage
- classes, langues et races préintégrés dans la fiche
- gestion des malus de charge selon le poid des objets
- glissé déposé du compendium Pathfinder pour les objets et les sorts ( avec conversion des unités et selon la catégorie de la taille si souhaité par l'utilisateur )
- pour le sorts gestion des échecs aux sorts profanes lors d'un lancé de dé, et prise en compte de la résistance magique

les fonctionnalitées secondaires :
- envoie des jet dans le chat ou uniquement au maître du jeu ( selon les options )
- utilisation de la fiche avec l'unité souhaitée ( longueur, distance et poids )
- au survol de chaque bouton pour lancr un dé; note sur le fonctionnement et formule utilisée
- conversion des longueur, poids, et taille selon la race et ses caractéristiques
- conversion des longueurs et poids lors d'un glissé-déposé du compendium ( selon les options )
- création d'une attaque a partir d'une arme de l'inventaire via un bouton
- transfert des armes, armures et objets dans un dépôt ( utile pour la gestion des charges transportables par un tier ou une mule )

note sur choix de design :
- beaucoup de fiches de créatures du compendium contiennent des erreurs, qui sont pour la plupart gérées par la fiche de personnage, exepté un cas ; le mauvais type de données dans la fiche ( par exemple "-" ou "&Mdash;" au lieu d'afficher des nombres ) 
- le ciblage n'est pas utilisé, car ça alourdi énormement la gestion des tests, même si d'un premier abord ca simplife les calculs, dans la pratique ca demande beaucoup de click, et en cas de multi-ciblage le système est caduc.
- associer des valeurs à un token ne peuvent être changé sur le token, ( santé, status des niveaux négatif ou status de somnolence par exemple ); leurs valeurs doivent être changées dans la fiche du personnage ( ceci est dû à une mécanique interne de roll20 ne pouvant être changé ).

## Modifications des règles Pathfinder
certains points sont discutables pour l'interprétation de certaines règles, voici la liste de ce qui a été décidé :

- le port d'armure n'est pas clair sur qui est impacté, donc :
  - avec formation : jet de compétences basés sur la force ou la dextérité
  - sans formation : jet de compétences basés sur la force ou la dextérité + initiative + jet d'attaque + DMD + reflexes
- les status concernant la CA ne sont pas toujours clair dans les règles quand la CMD est concernée ou pas, dans le doute sans mention contraire le status est appliqué à la CA et la DMD, sauf pour l'encombrement qui n'est pas appliqué à la CA.
- combattre sur la défensive peut se faire en action simple ou complexe, vu la similitude dans les règles, seul la CA (esquive) est impactée
- le poids transportable utilise un tableau pour déterminer ses valeurs, qui montre de gros soucis passé les 29 de force ( à calculer et a être cohérent ), par simplicité la formule suivante a été retenue. 
  > Force compris entre 0 à 10 : 5 x Force
  
  > Force supérieur à 10 : 24.9087 e^( 0.1386 x Force )
 
- L'âge limite pour la jeunesse n'est pas défini clairement dans les règles, donc les personnages auront les malus de la jeunesse tant qu'ils n'ont pas atteient l'âge adulte.

- le poids des objets peuvent varier de ce qui est trouvable sur le wiki français, les poids du compendium étant en pieds pour passer en kilogramme les valeurs sont converties puis arrondi au 0.5 le plus proche. Par exemple: 6 lb = 2.71 kg -> 2.5kg ( et non 3 kg comme on peut le trouver dans le wiki français); à noter que les poids totaux sont de cette façon plus cohérent et évite de froler la surcharge en se basant sur le wiki français.

## Modification et informations au sujet de la fiche
  - un erreur existe pour l'ordre du champ déroulant pour la vitesse de déplacement ( options ) -> raison non trouvée
  - une erreur existe pour l'ordre de la liste des écoles pour les sorts ( magie ) -> raison non trouvée
  - dans les status de classes seul deux sont affichés car ils ont des liés complexes et donc ne peuvent être reproduits en tant que status personnalisés ( il doit en exister d'autre;  faire un retour pour des ajouts )
  
## Règles CSS
Compilation des règles CSS utilisable pour la mise en page.

- disposition flex
  flex-row
  flex-col flex-col2 flex-col3 ... flex-col11
  flex-col-auto
- input-group
    input-group
    input-group-text
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

- BUG
  - utiliser getAttr() pour un select utilisant *data-i18n-list* renvoie systématiquement le **premier élément** de la liste une fois ordonnée ( côté client le select est sur le bon élément )
- général
  - liaison champ avec un token, uniquement les input simples ( sans **disabled** ou valeurs calculées ) peuvent être lié à un token, la seule solution est d'utiliser du ECMAscript pour changer la valeur
- sheet.json
  - pour utiliser la traduction de **description**, utiliser **displaytranslationkey** ( **description** peut etre omis )
  - pour utiliser la traduction de **description**, utiliser **descriptiontranslationkey** (**description** doit avoir une valeur autre que null ou vide, utiliser par exemple **"foo"** )
- HTML
  - **ne pas** utiliser le terme **_max** ou **_maximum** pour les variables utilisés dans des calculs; des comportements aléatoires sont à prévoir ( par exemple "@{hitpoints_max}" renvoie toujours 0 ), les réserver aux inputs à lier avec les tokens.
  - pour un "radio" les inputs **doivent** se suivre dans le code et **doivent** avoir l'attribute value
  - les balises html5 dans leur majorité ne sont pas autorisés
  - les attributes "data" pour les balises sont supprimés
  - pour les fieldset **repeating_xxx"** et les boutons **act_** ne pas utliser des undescores pour le nommage de la classe, mais faisable pour les variables dans le repeating, par exemple "repeating_var1_var2_var3" -> "repeating_var1" + "var2_var3"
    
- ECMAscript
  - "<script data-type="text/worker>" est valide pour roll20 ( au lieu de <script type="text/worker"> ), utile pour un interpréteur ECMAscript pendant le dev.
  - utiliser removeRepeatingRow() ne déclenche pas les events **on("change:foo")** ni **on("remove:foo")**, compensable si suivit de **setAttrs(payload, {silent:true}, callback)** ou **setAttrs(payload, {silent:false}, callback)**
- CSS
  - les règles pour "rolltemplate" sont indépendants du "character sheet"
  - des styles sont appliqués pour ".ui-dialog .charsheet", mais appliquer un style à ".ui-dialog <child>" est automatiquement retiré, obligeant a être restrictifs sur les styles ( exemple width et height des inputs )
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
