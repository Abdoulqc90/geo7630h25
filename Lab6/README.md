## Laboratoire 6 et 7 : ArcGIS Online, Dashboard et Experience Builder – Intégration de données

**Étape 1: Importation des données dans ArcGIS Online**

**1** Chargement des données CSV:
```bash
https://sitewebbixi.s3.amazonaws.com/uploads/docs/20220107-donnees-ouvertes-8aa623.zip
```
**2** Filtrage des attributs avec `AttributeFilter`tout en excluant les entités n’ayant pas de longitude. On ajoute **-1** pour filtrer les stations qui ont des mauvaises coordonnées ou des coordonnées éronnées. 

---
![alt text](attributefilter1.png)
---

**3** Calcul des statistiques avec `StatisticsCalculator` pour calculer les arrivées et départs par station. On ajoute un `StatisticsCalculator` sur le champ *emplacement_pk_end* sur le port *Missing de sortie* puis grouper les données pour obtenir le total des arrivées par station.
---
![alt text](statisticcalculator1.png)
---
On repète le même processus pour les départs (*emplacement_pk_start*).
---
![alt text](statisticcalculator2.png)
---
**4**  Nettoyage et gestion des attributs (*AttributeManager*) pour les stations de Bixi.
Utiliser `AttributeManager` pour Conserver uniquement les attributs nécessaires et garantir la cohérence des noms de champs. Ensuite, renommer les champs pour faciliter l’intégration dans ArcGIS Online (AGOL).
---
![alt text](<attribute manager properties1.png>)
---

![alt text](<nettoyage des attributs1.png>)
---
**5**  Jointure des données avec `FeatureJoiner`

On ajoute un `FeatureJoiner` pour relier les stations aux données de départ par stations et ensuite on configure la clé de jointure sur l’identifiant unique des stations (*PK pour Primary Key*). 
---
![alt text](featurejoiner1.png)
---

Ajouter ensuite un autre `FeatureJoiner` enchaîné sur le port de sortie *"Joined"* pour joindre les sommes des arrivées par stations. Il est important d'ajouter un `logger` pour s'assurer que tout fonctionne bien. On ajoute par la suite un `AttributeManager` pour faire le ménage et renommer les attributs 
---
![alt text](featurejoiner2.png)
---
![alt text](attributemanager2.png)
---

Cette étape se résume comme suit: 
---
![alt text](etape1.png)
---

## Étape 2 : Exportation des données vers ArcGIS Online

**1** Accéder à ArcGIS Online et créer un dossier GEO7630 à travers le lien suivant:
```bash
https://uqam.maps.arcgis.com/home/index.html
```
---
![alt text](<AGOL Geo7630.png>)
---
**2** Connecter FME à `AGOL Writer` puis Créer un nouveau Feature Service dans le dossier "Geo7630" précédemment crée. On active `CREATE IF NEEDED` pour la mise à jour automatique.
---
![alt text](<writer AGOL.png>)
---

![alt text](<writer AGOL2.png>)
---

![alt text](<writer AGOL3.png>)
---

**3** Visualisation et exploitation des données dans ArcGIS Online
On va dans la page `contenu` cliquer sur notre Nouveau `FeatureLayer` puis sur `Ouvrir dans MapViewer`
---
![alt text](<ouverture dans mapviewer.png>)
---

## Étape 3 : Création d’une carte interactive (MapViewer)

