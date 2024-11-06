# Créer une table de Mesures

1. Depuis le portail ajouter une table : _Home > Enter Data_
2. Ajouter une colonne avec une ligne (sans valeur particulière), Appeler la table _Calculs_, et valider
3. Dans cette table, créez votre première mesure : Clic droit sur la table _Calculs_ > Nouvelle mesure > Dans la barre de formule > _Today = TODAY()_ > Entrée
4. Supprimer la colonne de la table Calculs. La table devrait apparaître en-tête de la liste des tables, et son logo devrait changer pour une calculatrice.

# Créer ses premières mesures 

1. Nombre de stations
2. Nombre de vélos disponibles
3. Nombre de vélos mécaniques disponibles
4. Nombre de stations pleines
5. Nombre de stations vides
6. Taux de remplissage des stations
  - Pour effectuer une division, favoriser l'usage de la fonction DIVIDE( A, B), plutôt que "/" qui ne gère pas la division par zéro et qui peut générer des latences (CallBack)
  - Pour afficher le taux sous forme de %, cliquer sur la mesure et dans l'onglet Measure Tools, cliquer sur "%"
  - ![image](https://github.com/user-attachments/assets/c2d915fb-0721-4446-86c3-42540c9fe64c)
7. Pourcentage de stations pleines
8. Nombre de station dont la capacité est supérieure à 35
9. Rang de station en terme de capacité
10. Moyenne du nombre de vélos par stations
11. Booléen qui indique s'il y a des vélibs electriques ou non dans la borne.
12. Pourcentage de capacité que contient la station pour toutes les capacités disponibles

# Réponses
1. ```Nombre de stations = COUNTROWS(Stations)```
2. ```Nombre de vélos disponibles = SUM(Etats[Total Available Bikes])```
3. ```Nombre de vélos mécaniques disponibles = SUM(Etats[Mechanical Bikes Available])```
4. ```
    Nombre de stations pleines = CALCULATE(
        [Nombre de stations]
        , 'Etats'[Total Available Docks] = 0 
        , 'Etats'[Total Available Bikes] <> 0 
    )
5. ```
    Nombre de stations vides = CALCULATE(
        [Nombre de stations]
        , 'Etats'[Total Available Bikes] = 0 
    )
6. ```
    Taux de remplissage des stations = DIVIDE(
        [Nombre de vélos disponibles]
        , MAX(Stations[Capacity])
    )
7. ```
    Pourcentage de stations pleines = DIVIDE(
        [Nombre de stations pleines]
        , [Nombre de stations]
    )
8. ```
    Nombre de Stations Capacité Elevée = CALCULATE(
        [Nombre de stations]
        , Stations[Capacity] > 35
    )
9. ```
   Capacité Station = MAX(Stations[Capacity])
   Rang Capacité Station = RANKX( ALL( Stations ), [Capacite Station] ,,,DENSE)
10. ```Moyenne du nombre de vélos par stations = AVERAGE(Etats[Total Available Bikes])```
11. ```Vélos Electrique disponibles = IF(SUM(Etats[Eletric Bikes Available]) > 0, true, false)```
12. ```
    Pourcentage vs Total = DIVIDE(
        SUM(Stations[Capacity])
        , CALCULATE( SUM(Stations[Capacity]), ALL(Stations) )
    )

# Scenario What-If

1. Créer une table avec une liste de valeurs de 0 à 10
2. Cette table sans relation est dite "déconnectée". 
3. Créer une mesure ScenarioDisponibilite qui récupère la valeur selectionnée par l'utilisateur
4. Créer une mesure qui soustrait au nombre de vélo disponible la mesure ScenarioDisponibilite
5. Créer une visel filtre avec comme choix la colonne de la table déconnectée 
6. Dans une matrice, afficher le nom des stations, le nombre de vélo disponible et cette nouvelle mesure testée à l'étape 4. Jouer avec le filtre pour faire varier la mesure.
