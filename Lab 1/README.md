# GEO7630_LAB1: PRISE EN MAIN DES OUTILS 🛠️.

# ETAPE 1 : Création d’un compte GITHUB
Sur **GITHUB.COM**, on crée un nouveau compte et un nouveau repository nommé `geo7630h25`.

https://github.com/Abdoulqc90/geo7630h25/blob/main/Lab%201/README.md

Ouvrir ensuite **VS Code** puis configuration à l’aide de la commande `ctrl+j`

Entrer respectivement les noms d’utilisateurs et adresse courriel comme suit :
```bash
git config --global user.name "Abdoulqc90
```
```bash
git config --global user.email meite.bamo_abdoul_razak@courrier.uqam.ca
```
Puis valider.

Clonage du dépôt crée à l’aide de la commande `clone Git repository`.

Création d’un dossier **Lab1** dans lequel on crée un fichier `Readme.md`. On écrit **Geo7630** dans ce fichier Readme.md.

Enregistrer les modifications dans `contrôle de code source` en appuyant sur la touche **+**

Ensuite `committe et push` le tout vers GitHub.


# ETAPE 2 : lancer FME

Ouvrir **FME Workbench** puis  créer un nouveau `workspace`.

Dans `Readers`, puis `add reader`, on définit le format de fichier à importer sur **CSV**.

Dans `dataset`, on sélectionne `specify URL` puis on copie et colle le lien suivant :
```bash
https://donnees.montreal.ca/dataset/c1d65779-d3cb-44e8-af0a-b9f2c5f7766d/resource/28a4957d-732e-48f9-8adb-0624867d9bb0/download/businesses.csv
```

# ETAPE 3 : Injecter la table dans le schéma PostgreSQL

Configurer un `Writer` dans lequel on sélectionne le format `PostGIS` puis on sélectionne la connexion `geo7630h25`.

Dans `paramètres de connexion`, on spécifie la connexion à la base de données **Amazon** en copiant et collant le lien suivant dans *Host* :
```bash
geo7630h25.cvwywmuc8u6v.us-east-1.rds.amazonaws.com
```
Le code permanent est utilisé comme nom d’utilisateur et mot de passe.

Pour transformer les colonnes `latitude/longitude` en géométrie, on ajoute un `transformer VertexCreator`. Dans `Transformers`, on sélectionne `VertexCreator`. Puis dans les paramètres, on définit les valeurs de **X** sur **longitude** et **Y** sur **latitude**. 
Le système de coordonnées étant définit sur **EPSG:4326**.

Dans le Reader précédemment crée, dans `édit business [CSV2] parameters`, on va changer l’encodage en **Unicode 8-bit (utf-8)** afin de corriger les erreurs dans les attributs.

On Injecte la table CSV en tant que nouvelle table dans le schéma public avec le nom : **CODEMS_TABLE_1**.

![alt text](<FME screenshot.png>)

On vérifie que la table est bien présente à l’aide de l’application **DBeaver** : 

Ouvrir **DBeaver** puis créer une nouvelle connexion `PostgreSQL` tout en utilisant le code permanent comme identifiant et mot de passe. 

Une fois la connexion établie, on verifie que la table est bien présente tout en s’assurant que les géométries sont bien reconnues.

![alt text](<Table DBeaver.png>)

# ETAPE 4 : Visualiser la table dans QGIS

Ouvrir **QGIS** puis se connecter à la base `PostgreSQL` en créant une nouvelle connexion avec le code permanent comme identifiants.

La table importée est maintenant prête à être visualisée. 
Dans `propriétés de la couche`, on modifie la symbologie pour donner un style à notre couche de points.

Puis on sauvegarde le style dans la base de données en l’enregistrant par défaut en tant que `fichier de style de couche QGIS`.  Le style est enregistré dans `datasource database` et le nom du style est : 

**MEIB19269006_TABLE_1**.

# Validation : 
on vérifie que les points sont correctement positionnés selon les coordonnées de `latitude/longitude` puis on visualise la carte finale.

![alt text](<carte final.png>)






 







