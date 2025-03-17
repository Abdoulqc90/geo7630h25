#**LABORATOIRE 4: Intégration Matricielle FME + QGIS**

## Etape initiale: ajout des fichiers et création de readers.

```bash
![alt text](hm-2002-can-4000-0257_georeferenced.png) 
```

```bash
![alt text](ilots-de-chaleur-2016-1.tif)     
```

```bash
![alt text](MNS(terrain+batiment)_2015_1m_Ville-Marie.tif)  
```
---       

Choisir le `PNG Reader` pour le fichier PNG (image aérienne) et le `COG (cloud-optimized Geo TIFF)` pour les fichiers TIFF ( ilôts de chaleur et modèle numérique de surface). 
Creer un **bookmarks** pour chacun.

![alt text](bookm3.png) 

![alt text](<book 1.png>) 

![alt text](bookm2.png)


## Partie 1 - Intégration d’image aérienne standard

**Etape 1:** Reprojection du raster en `EPSG:32188`

![alt text](reproject1.png)
---

**Etape 2:** Utiliser le `RasterPropertyExtractor` pour extraire des propriétés et des métadonnées d'un raster.

![alt text](<raster prop esxtractor1.png>)
---

**Étape 3:**  utiliser le `RasterResampler` pour redimensionner ou rééchantillonner une image raster, ajustant ainsi sa résolution, sa taille ou sa géométrie.

![alt text](<raster resamplervrai1.png>)

![alt text](<raster resamplervrai1 arithmetic.png>) 
---

**Étape 5:** ajouter le `rasterPyramider` pour optimiser l'affichage et l'analyse de grandes images sur différents niveaux de zoom.

![alt text](rasterpyramid1.png)
---

**Étape 6:**  on utilise le `FeatureWriter` avec le nom de table: 

```bash
hm-2002-can-4000-0257.
```

 Contrairement au Writer de base celui ci permet de chaîner les actions à la suite de l’écriture.

![alt text](featurewriter.png)
---

**Étape 7:** dans `SQLExecuter` on exécute la requête suivante:

```bash
CREATE TABLE VOTRECODEMS.mns_pyramid_lvl_2 AS SELECT * FROM "hm-2002-can-4000-0257_pyramide" WHERE "_pyramid_level" = 2
```

![alt text](<sql executor.png>)

Le **workbench** se présente comme suit:





**Etape 8:** visualisation dans QGIS

---

# Partie 2 - Intégration de raster analytique - Ilôts de chaleur

**Etape 1:** Reprojection des îlots de chaleur en `32188`

![alt text](image-9.png)

---

**Étape 2:** ajouter un `RasterToPolygonCoercer` pour convertir des images raster en entités vectorielles polygonales. On change  le `Label Attribute` pour `classification`.

![alt text](image-10.png)

---

**Étape 3:** ajouter un `writer  Postgis` avec pour Nom de la table : `ilots_chaleur_polygones`.

![alt text](image-11.png)

---

**Étape 4:** Visualisation dans QGIS et changer la valeur de la symbologie pour `classification`.

![alt text](image-13.png)

---

![alt text](image-12.png)

---

**Étape 5:** utiliser le `RasterDiffuser` pour améliorer la netteté de l'image raster. Cela permet de rendre l'image plus claire et plus définie.

![alt text](image-14.png)

---

**Étape 6:** ajouter à la suite un `RasterCellValueRounder` avec la valeur **1** dans le champ `Decimal places`. Puis un `RasterToPolygonCoercer` avec pour label `classification`.

![alt text](image-15.png)
---

![alt text](image-16.png)
---

**Étape 7:** ajouter un **writer Postgis** avec pour nom de table ` _ilots_chaleur_polygones_sharp` et ensuite visualisation dans QGIS.

![alt text](image-17.png)
---

![alt text](image-18.png)
---

**Étape 8:** utiliser  `RasterCellCoercer` 

![alt text](image-19.png)
---

**Étape 9:** utiliser ensuite un **writer Postgis** avec le nom de la table `ilots_chaleur_points`. Puis visualisation dans QGIS. Dans symbologie, utiliser **$z** pour extraire la valeur **Z** dans le `constructeur d'expression`.

![alt text](image-20.png)
---

![alt text](image-21.png)
---

![alt text](image-22.png)
---

Le **Workbench** se presente comme suit:

![alt text](image-23.png)
---

# Partie 3: Intégration de raster (MNS)**

**Étape 1:** ajoiuter un `ContourGenerator` pour créer des lignes de contour à partir d'une surface raster.

![alt text](image-24.png)
---

**Étape 2:** ajout d'un `Generalizer` pour simplifier ou lisser la géométrie des entités en réduisant le nombre de sommets. 

![alt text](image-25.png)
---

**Étape 3:** utiliser `AreaBuilder` pour créer des polygones (zones) à partir de lignes ou de points.

![alt text](image-26.png)
---

**Étape 4:** ajout d'un **writer (Postgis)** avec pour nom de table  `mns_contour_polygones`

![alt text](image-27.png)
---

le **Workbench** se présente comme suit:

![alt text](image-28.png)
---

**Étape 5:** effectuer la Visualisation dans QGIS

![alt text](image-29.png)
---

![alt text](image-30.png)
---
---





