# Relations entre les tables 

Pour ce lab, nous allons utiliser un modèle dont la transformation est déjà réalisée. Nous allons adresser la partie modélisation uniquement. Commençons par télécharger le fichier [a relative link](WorkshopSalesAnalysis No Relationships.pbix)

# Diagramme, Ordre des valeurs et Hiérarchie 

Pour continuer sur ce lab, si la partie précédente n'a pas été facile, vous pouvez repartir du modèle qui a déjà été développé appelé [WorkshopSalesAnalysis.pbix](WorkshopSalesAnalysis.pbix) disponible à la racine du Git. 

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
    1. ![image](https://github.com/user-attachments/assets/8665c1c0-fd26-4331-8f2d-d5e4701fffc0)

# Utiliser une relation inactive dans un calcul

1. Essayons d'afficher le nombre de ventes par Mois. D'après le diagramme, fonctionnellement, par quel date de la table des ventes la mesure est elle aggrégée ?
    1. Si l'on veut pouvoir calculer les ventes par "Date de livraison" (ShipDate) plutôt que par "Date d'échéance" (DueDate), deux solutions s'offrent à nous : Essayons de comprendre quels sont les enjeux (filtres, aspect pratique ...) 
    2. Recréer une autre table de date, et avoir une relation active entre cette nouvelle table et la seconde colonne de la même table : si l'on recrée une autre table de date, il faut alors la maintenir pour supporter les mêmes informations, et faire comprendre qu'il y en a deux avec des rôles différents.
    3. Forcer l'usage de la relation lors des calculs
2. Une fonction appelée USERELATIONSHIP permet de forcer l'usage d'une relation (seulement si elle est inactive) dans le cas d'un calcul : https://learn.microsoft.com/fr-fr/dax/userelationship-function-dax
3. Pour l'utiliser, créer une deuxième mesure _TotalSalesShipDate_ en utilisant la fonction USERLATIONSHIP.
4. Réponse : ```TotalSalesShipDate = CALCULATE([TotalSales], USERELATIONSHIP('Date'[Date] , SalesOrderHeader[ShipDate] ))```
5. Répeter la logique pour _OrderDate_ :
6. Le résultat peut-être affiché sur la même Matrice, et le contexte diffère pour chaque mesure :
7. ![image](https://github.com/user-attachments/assets/333dd35d-e37e-401a-8738-402722210a62)


# Comprendre les mesures qui retournent une valeur ou une table 

Pour aller plus loin, il est important de comprendre que certaines fonctions retournent des tables (fonction tabulaires), et d'autres retournent des valeurs simples (fonction scalaires). Il est indiqué dans la documentation quel type de contenu une fonction peut retourner. 
Pour illustrer nos propos, nous allons créer une table calculée : elle diffère des tables issues de Power Query, car elles sont évalues en dernier, une fois que les données ont été importées. Il est important de ne pas avoir pour racourci de toujours créer ces tables en DAX plutôt qu'en Power Query : chaque contexte est différent. 
L'ordre de préférence pour créer une table calculée ou une colonne calculée est le suivant : 
1. Si je peux créer cette table à la source, je le fais
2. Sinon, si je peux créer cette table via Power Query, je le fais,
3. Enfin, si je dois la créer en DAX, alors je crée une table calculée.

## Les colonnes calculées 

Voici les étapes pour créer une colonne calculée : 
1. Cliquer sur la table SalesOrderDetail et choisir dans l'onglet **Table Tools**, **New Column**
2. Dans la barre de formule, calculer le prix total de vente
3. Réponse : ```TotalAmount = SalesOrderDetail[UnitPrice] * SalesOrderDetail[OrderQty]```
4. Vous remarquerez la différence de logo d'une colonne classique, d'une mesure, et d'une colonne calculée.
5. Essayez maintenant de créer la colonne calculée suivante ```TotalSumQuantity = SUM(SalesOrderDetail[OrderQty])```
6. Réflechissez à la différence entre une colonne calculée et une mesure : est-ce pertinent de représenter en dur la valeur concernant la somme totale des quantités pour toutes les lignes du tableau ?

## Les tables calculées

Voici les étapes pour créer une table calculée : 
1. Dans le menu **Modeling**, cliquer sur **New Table**
2. Créer une mesure permettant de ne filtrer que les SalesOrderDetails qui ont des produits rouges :
    1. ```RedProducts = CALCULATETABLE(SalesOrderDetail, Product[Color] = "Red") ```
3. La table calculée que nous venons de créer pourrait être utilisée directement au sein d'une mesure : Ne calculer la somme des produits vendus que pour les produits rouges :  
    1. ```TotalSalesRedProducts = SUMX(CALCULATETABLE(SalesOrderDetail, Product[Color] = "Red"), SalesOrderDetail[UnitPrice] * SalesOrderDetail[OrderQty])```
4. Idem, les logos des tables calculées, tables de mesures, et tables classique est différent. 
 
Créons maintenant une table aggrégée. Pour cela, nous utiliserons la fonction SUMMARIZE(table, colonne d'aggrégat, "nom colonne aggrégée", fonction d'aggrégat) https://learn.microsoft.com/fr-fr/dax/summarize-function-dax
1. Dans le menu **Modeling**, cliquer sur **New Table**
2. Utiliser la fonction SUMMARIZE pour aggréger la quantité de produits par Numéro de commandes de la table SalesOrderDetails
3. Réponse : ```QuantityPerOrders = SUMMARIZE(SalesOrderDetail, SalesOrderDetail[SalesOrderID], "SumQuantity", SUM(SalesOrderDetail[OrderQty]))```

Les fonctions peuvent être combinées pour retourner des tables aggrégée, mais aussi bien filtrées. 
1. Créons la même table mais seulement pour les produits rouges
2. Réponse : ```QuantityPerOrdersWithRedProducts = CALCULATETABLE(SUMMARIZE(SalesOrderDetail, SalesOrderDetail[SalesOrderID], "SumQuantity", SUM(SalesOrderDetail[OrderQty])), Product[Color] = "Red" ) ```

Consultons alors le modèle : cette table peut-elle être analysée par date ? 

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
            SalesCountYearToDate = CALCULATE(
                [SalesCount]
                , DATESYTD('Date'[Date])
            )
        3. Cette mesure existe en Month To Date _DATESMTD()_, Quarter To Date _DATESQTD()_, Fiscal Year to date _TOTALYTD()_ avec une définition de la date de départ ...
    3. Maintenant que l'on a compris que la Time Intelligence facilitait le travail via des fonctions déjà intégrées, réflichir à un moyen de calculer la différence des ventes vis à vis de l'année précédente.
        1. Il est possible de le faire en créant une seconde mesure qui calcule le nombre de ventes à l'année N-1, puis en soustrayant cette valeur dans une autre mesure.
        2. Cette fois-ci, il faut utiliser une mesure qui permet de renvoyer une table. cette table contient les dates de la même période à l'année N-1 pour évalue la même mesure. Cette fonction est la fonction **SAMEPERIODLASTYEAR(Date)**.
        3.  ``` 
            SalesCountLastPeriod = CALCULATE(
                [SalesCount]
                , SAMEPERIODLASTYEAR('Date'[Date])
            )
        4. Affichons cette mesure dans une table, à côté de la mesure [SalesCount] : on retrouve les valeurs de décembre 2022 à côté de celles de 2023
         ![image](https://github.com/user-attachments/assets/f9f9657b-9242-435b-be49-d73664c91355)

        6. Maintenant, il ne nous reste plus qu'à soustraire l'une et l'autre, et l'on saura s'il on a été plus ou moins performant vis à vis de cette période :
            1.  ``` VersusLastPeriod = [SalesCount] - [SalesCountLastPeriod]

# Les variables 

L'utilisation de variables est très important pour des soucis de performances ou de lisibilité. Dans le cadre d'un test, Power BI exécutera le test pour le résultat. Dans le cas ou l'on ne veut afficher un résultat que dans un cas ou un autre, l'utilisation d'une variable permet de ne faire le calcul qu'une seule fois plutôt que deux : 
1. Sans variable : ``` DummyTest = IF([VersusLastPeriod] > 0, [VersusLastPeriod], BLANK())  ```
2. Avec variable :
   ```
    DummyTest = 
        VAR vTest = [VersusLastPeriod]
    RETURN 
        IF(vTest > 0, vTest, BLANK())

+ Lab sur les valeurs évaluées avant les autres mesures
+ Lab sur les variables pour nettoyer les résutlats. 
   
# Utiliser les Calculation Groups

# RLS 
