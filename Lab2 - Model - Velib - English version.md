# Create a Measure Table  

1. From the portal, add a table: _Home > Enter Data_  
2. Add a column with a single row (no specific value). Name the table _Calculations_ and validate.  
3. In this table, create your first measure: Right-click on the _Calculations_ table > New Measure > In the formula bar, enter _Today = TODAY()_ > Press Enter.  
4. Delete the column in the _Calculations_ table. The table should appear at the top of the list of tables, and its icon should change to a calculator.  

# Create Your First Measures  

1. Number of stations  
2. Number of available bikes  
3. Number of available mechanical bikes  
4. Number of full stations  
5. Number of empty stations  
6. Station fill rate  
   - To perform a division, use the `DIVIDE(A, B)` function instead of `/`, as `DIVIDE` handles division by zero and reduces latencies (CallBack).  
   - To display the rate as a percentage, click on the measure and, in the _Measure Tools_ tab, click "%".  
   - ![image](https://github.com/user-attachments/assets/c2d915fb-0721-4446-86c3-42540c9fe64c)  
7. Percentage of full stations  
8. Number of stations with a capacity greater than 35  
9. Station rank in terms of capacity  
10. Average number of bikes per station  
11. Boolean indicating whether electric bikes are available at the station  
12. Percentage of the station's capacity compared to the total available capacity  

# Answers  

1. ```Nombre de stations = COUNTROWS(Stations)```  
2. ```Nombre de vélos disponibles = SUM(Etats[Total Available Bikes])```  
3. ```Nombre de vélos mécaniques disponibles = SUM(Etats[Mechanical Bikes Available])```  
4. ```  
   Nombre de stations pleines = CALCULATE(  
       [Nombre de stations],  
       'Etats'[Total Available Docks] = 0,  
       'Etats'[Total Available Bikes] <> 0  
   )  
   ```  
5. ```  
   Nombre de stations vides = CALCULATE(  
       [Nombre de stations],  
       'Etats'[Total Available Bikes] = 0  
   )  
   ```  
6. ```  
   Taux de remplissage des stations = DIVIDE(  
       [Nombre de vélos disponibles],  
       MAX(Stations[Capacity])  
   )  
   ```  
7. ```  
   Pourcentage de stations pleines = DIVIDE(  
       [Nombre de stations pleines],  
       [Nombre de stations]  
   )  
   ```  
8. ```  
   Nombre de Stations Capacité Elevée = CALCULATE(  
       [Nombre de stations],  
       Stations[Capacity] > 35  
   )  
   ```  
9. ```  
   Capacité Station = MAX(Stations[Capacity])  
   Rang Capacité Station = RANKX(ALL(Stations), [Capacite Station], , , DENSE)  
   ```  
10. ```Moyenne du nombre de vélos par stations = AVERAGE(Etats[Total Available Bikes])```  
11. ```Vélos Electrique disponibles = IF(SUM(Etats[Electric Bikes Available]) > 0, true, false)```  
12. ```  
    Pourcentage vs Total = DIVIDE(  
        SUM(Stations[Capacity]),  
        CALCULATE(SUM(Stations[Capacity]), ALL(Stations))  
    )  
    ```  

# What-If Scenario  

1. Create a table with a list of values from 0 to 10.  
2. This table, without any relationship, is referred to as a "disconnected" table.  
3. Create a measure `ScenarioDisponibilite` that retrieves the value selected by the user:  
   1. Use the `MIN()`, `MAX()`, or `SELECTEDVALUE()` function.  
4. Create a measure that subtracts the `ScenarioDisponibilite` value from the number of available bikes.  
5. Create a slicer with the column from the disconnected table as the choice.  
6. In a matrix, display the station names, the number of available bikes, and the new measure created in step 4. Play with the slicer to test the measure.  

--- 

Let me know if you'd like any refinements!
