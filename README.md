# pathfinder
template for roll20 pathfinder

Remarques concernant roll20 et la création du "character sheet" :

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
