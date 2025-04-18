#**Laboratoire 5: Intégration et visualisation de données 3D Lidar et tuiles 3D Vectorielles**

## Étape 1 Importation et nettoyage des nuages de points.

Utiliser un `LAS READER` pour l'ajout de 6 nuages de points puis ajouter un `Thinner` avec un filtre de 30 pour chaque reader. Pour combiner les 6 nuages de points en 1 seul, on ajoute un `pointcloudcombiner` et un `EsriReprojector` (2950 to 3857)pour reprojeter les points.
----

```bash
http://depot.ville.montreal.qc.ca/geomatique/lidar_aerien/2015/301-5041_2015.laz
```

```bash
http://depot.ville.montreal.qc.ca/geomatique/lidar_aerien/2015/301-5040_2015.laz
```

```bash
http://depot.ville.montreal.qc.ca/geomatique/lidar_aerien/2015/301-5039_2015.laz
```

```bash
http://depot.ville.montreal.qc.ca/geomatique/lidar_aerien/2015/300-5041_2015.laz
```

```bash
http://depot.ville.montreal.qc.ca/geomatique/lidar_aerien/2015/300-5040_2015.laz
```

```bash
http://depot.ville.montreal.qc.ca/geomatique/lidar_aerien/2015/300-5039_2015.laz
```
----

![alt text](<Thiner parameters.png>)

---

![alt text](pointcloudparameters.png)

---

![alt text](<Esri reprojector parameters.png>)

---
La prémière étape se présente comme suit: 
----
![alt text](etape1.png)

-----
## Étape 2: Importation des limites terrestres et découpage du nuage de points.
---
**1** Ajout d'un `GeoJSONReader` (URL) puis un `Reprojector` 4326 vers 3857 

```bash
https://data.montreal.ca/dataset/b628f1da-9dc3-4bb1-9875-1470f891afb1/resource/92cb062a-11be-4222-9ea5-867e7e64c5ff/download/limites-terrestres.geojson
```
-----

![alt text](<reprojector etape2.png>)
----

**2** On utilises un `Clipper` pour découper le nuage de points puis avec la limites terrestres pour enlever les points superflus qui dépasse de notre zone de travail

----
![alt text](<clipper parameters etape2.png>)
----