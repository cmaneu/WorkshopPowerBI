Dans ce Lab nous allons importer les données des APIs Vélib. 

# Création des tables

Deux URLs seront utilisées pour accéder à ces données : 
1. https://velib-metropole-opendata.smovengo.cloud/opendata/Velib_Metropole/station_information.json
2. https://velib-metropole-opendata.smovengo.cloud/opendata/Velib_Metropole/station_status.json

Pour importer les données de ces APIs nous allons Récupere les données via le Connecteur Web : 
1. Get Data > More ... > Web > Connect 
2. Coller l'URL dans l'invite 
3. Répéter l'étape depuis Power Query pour obtenir les deux requêtes côte à côte 
4. Renommer les deux requêtes en utilisant la partie proriété sur la droite OU en cliquant sur le nom de la Query et utilisant "F2", ou via Click droit > Rename :
  - station_status > Etats
  - station_information > Stations

# Optionnel : Décortiquer le fichier JSON

## Stations
1. Supprimer les étapes générées par le moteur Power Query sur la droite en retirant les étapes les unes après les autres 
2. Convertir le document en tableau exploitable 
3. Transposer les lignes en colonnes
4. Utiliser la première ligne comme en-tête
5. Exposer les données de la colonnes Data grâce au bouton "Expand" situé à côté du nom de la colonne. Ne pas conserver le prefixe de la colonne. 
6. Etendre la liste "stations" en utilisant le même bouton. En consultant le lien des stations (voir partie 1, 1) ), essayez de déterminer si l'expositon doit être faite sur les valeurs ou sur de novuelles lignes. 
7. Après avoir réfléchis, choisissez sur de nouvelles lignes. 
8. Exposer le contenu des Lignes : 
  - Retirez le prefixe
  - Lister l'ensemble des colonnes disponibles

## Etats
1. Supprimer les étapes générées par le moteur Power Query sur la droite en retirant les étapes les unes après les autres 
2. Convertir le document en tableau exploitable 
3. Transposer les lignes en colonnes
4. Utiliser la première ligne comme en-tête
5. Exposer les données de la colonnes Data grâce au bouton "Expand" situé à côté du nom de la colonne. Ne pas conserver le prefixe de la colonne. 
6. Etendre la liste "stations" en utilisant le même bouton. En consultant le lien des stations (voir partie 1, 1) ), essayez de déterminer si l'expositon doit être faite sur les valeurs ou sur de novuelles lignes. 
7. Après avoir réfléchis, choisissez sur de nouvelles lignes. 
8. Exposer le contenu des Lignes : 
  - Retirez le prefixe
  - Lister l'ensemble des colonnes disponibles 

# Nettoyage des données :
## Stations
1. Nous allons nettoyer les données et préparer l'analyse dans notre rapport. N'hésitez pas à compléter ces étapes avec ce qui vous semble pertinent. 
2. Retirez les colonnnes lastUpdatedOther et ttl, en utilisant "Ctrl + Click" sur les deux colonnes, puis click droit et Retirer les colonnes. 
3. En consultant l'URL des stations_status (voir partie 1, 2) ), déterminez si l'usage des colonnes stationCode et station_Id est pertinant. Gardez la colonne la plus performante. 
4. Rennomez les colonnes (en déduisant la colonne que vous avez retiré à l'étape 3) : 
  - station_id > Station Id
  - stationCode > Station Code
  - name > Name
  - lat > Latitude
  - lon > Longitude
  - capacity > Capacity
5. Déduisez le typage des colonnes pertinent selon chaque informations. Utilisez si bon vous semble la fonctionnalité "Detect data types"

## Etats
1. Nous allons nettoyer les données et préparer l'analyse dans notre rapport. N'hésitez pas à compléter ces étapes avec ce qui vous semble pertinent. 
2. Retirez les colonnnes lastUpdatedOther et ttl, en utilisant "Ctrl + Click" sur les deux colonnes, puis click droit et Retirer les colonnes. 
3. Gardez la colonne la plus performante en suivant votre logique du nettoyage des information des stations. 
4. Retirez les colonnes en double dans la table. 
5. Renommez les colonnes :
  - num_bikes_available ou numBikesAvailable > Available Bikes 
  - num_docks_available ou numDocksAvailable > Available Docks
  - is_Installed > Installed Flag
  - is_returning > Returning Flag 
  - is_renting > Renting Flag
  - last_reported > Last Reported Date

6. Utilisez la fonctionnalité de transformation/ajout de colonnes par la méthode de votre choix pour transformer les colonnes "... Flag" en colonnes true/false. 
7. Changez ensuite le type de la colonne en type true/false 
8. Trouvez un moyen de de transformer la colonnes last_reported en valeur au format Datetime. 

## General 
Il est important de prendre du recul une fois les étapes appliquées. L'ordre des étapes est important : 
- Plus les étapes appliquées sont nombreuses, et plus l'ordre est important
- Les étapes les plus discriminantes doivent être faites en premier : les filtres, suppression de colones ...
- Les étapes qui se répètent peuvent être unifiées et mises ensemble (renommer toutes les colonnes d'un coup) 

# Paramétrage des requêtes 

Nous allons découvrir l'usage des paramètres pour simplifier l'évolution du tableau en remplaçant l'URL fixe par un paramètre.   
1. Créer un nouveau paramètre appelé "URL" via l'une des deux méthodes suivantes : 
  - Click Droit sous Query > New Parameter ... 
  - Onglet Home > Manage Parameter > New Parameter
2. Définir le nom sur "URL"
3. Définir le type sur Text 
4. Remplir la valeur dans "current value" avec ce qui est en commun dans les deux URL proposées plus haut. 
5. Pour chaque requête Stations et Etat, répéter les étapes suivantes : 
  - Cliquer sur la Query choisie 
  - Dans les étapes appliquées, retrouver Source 
  - Dans la barre de Formule, remplacer la partie fixe par le paramètre URL, et concatener avec le reste via l'usage de URL & "string" (remplacer string par la bonne valeur)
  - Cliquer sur la dernière étape et valider le développement. 

# Appliquer les développements dans la partie Power BI 

Une fois les développement effectués, appliquer les développements dans la partie Power Query : 
1. Dans l'onglet Home, cliquer sur Appliquer les changements. 

# Table Date


