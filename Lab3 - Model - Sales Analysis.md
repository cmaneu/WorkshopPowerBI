# Diagramme, Ordre des colonnes et Time Intelligence 

1. Pour s'approprier le modèle de données, il convient de regarder dans un premier temps le volet **Model**, sur la droite. On peut alors comprendre les relations entre les tables et les impacts des filtres ou des mesures à venir.
2. Pour se faciliter la tâche, il est possible de réorganiser les tables, de les réduire/étendre, ou de créer un second diagramme et d'y ajouter les tables que l'on veut. 
  - ![image](https://github.com/user-attachments/assets/80ae3b51-ed7b-4b14-a33e-71b97b9fd8dd)
  - ![image](https://github.com/user-attachments/assets/b274da68-304d-4df4-929f-d2ddbc086aab)
  - ![image](https://github.com/user-attachments/assets/a1f493fa-577f-4a0e-8257-187f5fa21e6d)

3. Créons un premier visuel permettant d'afficher la mesure [SalesCount] par

4. Dans la partie Model, 

5. Définir comme table date

6. Créer une hiérarchie de date 

# Utiliser une relation inactive dans un calcul

1. Essayons d'afficher le nombre de ventes par Mois. D'après le diagramme, fonctionnellement, par quel date de la table des ventes la mesure est elle aggrégée ?
  - Si l'on veut pouvoir calculer les ventes par "Date de livraison" (ShipDate) plutôt que par "Date d'échéance" (DueDate), deux solutions s'offrent à nous : Essayons de comprendre quels sont les enjeux (filtres, aspect pratique ...) 
    - Recréer une autre table de date, et avoir une relation active entre cette nouvelle table et la seconde colonne de la même table
    - Forcer l'usage de la relation lors des calculs
  - Si l'on recrée une autre table de date, il faut alors la maintenir pour supporter les mêmes informations, faire comprendre qu'il y en a deux avec des rôles différents, et enfin 
  - Une fonction appelée USERELATIONSHIP permet de forcer l'usage d'une relation (seulement si elle est inactive) dans le cas d'un calcul : https://learn.microsoft.com/fr-fr/dax/userelationship-function-dax
  - 

# Quelques mesures très utiles 

# Comprendre les mesures semi-additive (MeasureX)

# Utiliser les Calculation Groups

# RLS 
