# Diagramme, Ordre des valeurs et Hiérarchie 

1. Pour s'approprier le modèle de données, il convient de regarder dans un premier temps le volet **Model**, sur la droite. On peut alors comprendre les relations entre les tables et les impacts des filtres ou des mesures à venir.
2. Pour se faciliter la tâche, il est possible de réorganiser les tables, de les réduire/étendre, ou de créer un second diagramme et d'y ajouter les tables que l'on veut. 
    1. ![image](https://github.com/user-attachments/assets/80ae3b51-ed7b-4b14-a33e-71b97b9fd8dd)
    2. ![image](https://github.com/user-attachments/assets/b274da68-304d-4df4-929f-d2ddbc086aab)
    3. ![image](https://github.com/user-attachments/assets/a1f493fa-577f-4a0e-8257-187f5fa21e6d)

3. L'ordre des valeurs à l'affichage :
    1. Créons un premier visuel permettant d'afficher la mesure [SalesCount] par Mois : Utiliser le visuel Table : dans la Vue **Report**, Cliquer sur le fond blanc de la page. Dans la liste des visuels, choisir le visuel Table, puis choisir les champs Date\Month et DAX\SalesCount
    2. ![image](https://github.com/user-attachments/assets/b609f30c-a15d-4969-a9b2-83f1afb9c5ed)
    3. L'ordre d'affichage est fait par l'ordre alpha-numérique : D'abbord la date, puis le mois qui commence par un A, puis par un B, ...
    4. Il est possible de changer l'ordre d'affichage d'un visuel : En clicant sur le visuel, on voit apparaître **...** autour du visuel (en haut ou en bas à droite).
        1. Cliquer sur **...**, puis Sort By > Sales Count.
        2. L'ordre est alors fait par ordre croissant ou décroissant de ventes, mais pas par l'ordre des mois de l'année.
    5. Pour résoudre ce sujet, il est possible de faire appel à une autre colonne, appelée colonne de tri :
        1. Dans la Vue **Data**, choisir la table de date sur la droite.
        2. Cliquer su la colonne Month, et choisir dans l'onglet **Column Tools** _Sort By Column_.
        3. Choisir la colonne MonthKey, qui elle est numérique (et donc donne l'ordre des mois dans leur ordre calendaire plutôt qu'alphabétique).
        4. Attention : cette méthode ne marche que si les deux colonnes ont la même cardinalité ! (même nombre de valeurs distinctes et équivalence des valeurs pour les mêmes lignes)

5. Il est possible de faciliter le travail des utilisateurs en associant les champs entre eux, lorsqu'ils sont Parents/Enfants (ex : Categories de produits, Dates). Cela s'appelle une hiérarchie.
    1. Pour y arriver, dans nimporte quelle vue, cliquer droit sur un champ, puis en cliquant sur **Create Hierarchy**
    2. Dans la table Date, cliquer droit sur _Year_ puis créer une hiérarchie. Renommer cette hiérarchie en Année/Trimestre/Mois/Jour
    3. Les champs s'ajoutent dans l'ordre d'ajout à la hiérarchie, et pas nécessairement dans l'ordre voulu fonctionnellement. Il est possible de les réorganiser depuis la vue **Model**, en cliquant sur la héirarchie et en changeant l'ordre dans l'onglet Advanced :
    4. ![image](https://github.com/user-attachments/assets/e5d15c89-ebb1-4d49-9bdd-f40496a92647)

# Comprendre les mesures "ligne à ligne" (MeasureX)

Dans certains cas, il est impossible de calculer le résultat d'un calcul sans effectuer ce calcul ligne par lignes. Prenons l'exemple de la table **SalesOrderDetail** qui donne le détail de chaque factures avec le prix des produits et la quantité commandée. Pour obtenir le chiffre d'affaire total, il faut multiplier la quantité par le prix du produit vendu. Si l'on somme l'ensemble des produits commandés et que l'on multiplie par la somme des prix, le résultat ne sera pas bon : chaque produit a un prix différent, et la quantité doit être appliquée à la ligne. Pour y arriver, il existe des mesures qui calcule un résultat par ligne. Attention, elles consomment plus de ressources que leur équivalent sans X. 

1. Pour calculer le total, on va donc utiliser la mesure SUMX : 
    1. Syntaxe : ```SUMX(<table>, <expression>)```
    2. La table sera donc **SalesOrderDetail**, et l'expression sera alors [UnitPrice] * [OrderQty]
    3. Réponse : ```TotalSales = SUMX(SalesOrderDetail, SalesOrderDetail[UnitPrice] * SalesOrderDetail[OrderQty])```
2. Affichons le résultat de cette mesure dans une matrice, ventilée par mois par exemple :      

# Comprendre les mesures qui retournent une table 

Pour aller plus loin, il est 

# Time Intelligence

Pour permettre à Power BI de gagner en magie, il est possible d'activer uen fonctionnalité appelée Time Intelligence. Cette fonctionnalité permet l'usage de fonction avancée pour comparer des valeurs dans le temps (par rapport au début de l'année, à l'année précédente ...). Pour cela, le modèle doit avoir une table de date dont les valeurs sont contigues dans le modèle. 

1. Définir une table comme table date
    1. Pour y arriver, cliquer droit sur la table date, puis sur **Mark as Date Table**. Choisir le champs dont la granularité est la plus fine (ici _Date_)
    2. ![image](https://github.com/user-attachments/assets/afe29050-74a4-43aa-87cb-af69b49267b8)

2. Essayer les mesures de **Time Intelligence**.
    1. Créer un visuel Matrice, avec en lignes les **années** et les **mois** (champs ou composant de la hiérarchie) et en colonnes la mesure **[SalesCount]**
    2. Créer une nouvelle mesure permettant de calculer le nombre de ventes totale depuis le début de l'année, et l'afficher dans le diagramme
        1. Utiliser la fonction Calculate pour changer la manière dont la mesure est filtrée, et la fonction DATESYTD qui permet de retourner l'ensemble des dates depuis le début de l'année 
        2. ``` 
            CALCULATE(
                [SalesCount]
                , DATESYTD('Date'[Date])
            )
        3. Cette mesure existe en Month To Date _DATESMTD()_, Quarter To Date _DATESQTD()_, Fiscal Year to date _TOTALYTD()_ avec une définition de la date de départ ...
    3. Maintenant que l'on a compris que la Time Intelligence facilitait le travail via des fonctions déjà intégrées, réflichir à un moyen de calculer la différence des ventes vis à vis de l'année précédente.
        1. Il est possible de le faire en créant une seconde mesure qui calcule le nombre de ventes à l'année N-1, puis en soustrayant cette valeur dans une autre mesure.
        2. Cette fois-ci, il faut utiliser une mesure qui permet de renvoyer une table. 

# Utiliser une relation inactive dans un calcul

1. Essayons d'afficher le nombre de ventes par Mois. D'après le diagramme, fonctionnellement, par quel date de la table des ventes la mesure est elle aggrégée ?
  - Si l'on veut pouvoir calculer les ventes par "Date de livraison" (ShipDate) plutôt que par "Date d'échéance" (DueDate), deux solutions s'offrent à nous : Essayons de comprendre quels sont les enjeux (filtres, aspect pratique ...) 
    - Recréer une autre table de date, et avoir une relation active entre cette nouvelle table et la seconde colonne de la même table
    - Forcer l'usage de la relation lors des calculs
  - Si l'on recrée une autre table de date, il faut alors la maintenir pour supporter les mêmes informations, faire comprendre qu'il y en a deux avec des rôles différents, et enfin 
  - Une fonction appelée USERELATIONSHIP permet de forcer l'usage d'une relation (seulement si elle est inactive) dans le cas d'un calcul : https://learn.microsoft.com/fr-fr/dax/userelationship-function-dax
  - 

# Quelques mesures très utiles 

# Utiliser les Calculation Groups

# RLS 
