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
- combattre sur la défensive peut se faire en action simple ou complexe, vu la similitude dans les règles, seul la CA (esquive) est impactée
- le poids transportable utilise un tableau pour déterminer ses valeurs, qui montre de gros soucis passé les 29 de force ( à calculer et a être cohérent ), par simplicité la formule suivante a été retenue. 
  > Force compris entre 0 à 10 : 5 x Force
  
  > Force supérieur à 10 : 24.9087 e^( 0.1386 x Force )
 
- L'âge limite pour la jeunesse n'est pas défini clairement dans les règles, donc les personnages auront les malus de la jeunesse tant qu'ils n'ont pas atteient l'âge adulte.

## Modification de la fiche prévues

- Global
  - changer navigation onglet selon [css wizardy](https://wiki.roll20.net/CSS_Wizardry)
  - manque la traduction(fr) "spell-concentration-pinned-label"
  - manque la traduction(fr) "status-effect-strength-ceiling"
  - manque la traduction(fr) "status-effect-dexterity-ceiling"
  - le status lié à l'age ne s'affiche pas
  - erreur ordre liste des écoles (racine)
  - mauvais ordre pour les sorts d'illusion
  - enlever title pour "modifier-status"
  
- Entête
    - pas d'info au survol pour max point de vie
    - status personnalisé : placeholder remplacer "forumule" par "formule"
    - status personnalisé : changer title en minuscule
    - le textes des boutons a un line-height mauvais
    
- Personnage

- Spécialisation

- Combat
  - BMO : changer title, les mot ne sont pas mis en majuscule, changer "+ Malus"  par " - |Malus|"
  - Attaque : changer title lancé de dé "+ Total" par "+ Bonus Atq."
  - Attaque : changer title BBA "+ Equipment" par "+ Divers"

- Magie
  - NLS : enlever title de "Status"
  - NLS : changer title du lancé de dé "+ Total + Status" par "+ Total"
  - risque échec aux sorts profanes : ajouter "Status" au title de total

- Défense
  - CA : ajouter pour standard, distane et dépourvu au title "+ Parade"
  - CA : pour contact ajouter "(ignore ... Parade)"
  - DMD : pour le title déplacer "status" avant "Base"

- Compétences
  - ordonnancement des compétences n'est pas actif ( erreur sur item "sabotage" )
  - manque "skill-class-competence-short"
  - si malus du à l'encombrement s'applique, l'ajouter pour les compétences custom si dext/force est utilisé
  - natation : title remplacer "Batation" par "Natation"
  - représentation : title remplacer "rprésentation" par "représentation"

- Inventaire
  - pour les boutons de déplacement d'objet, enlever les majuscules

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
