# Lab 11 – Cartographie interactive avec MapLibreGL

## Étape 1: création de fichiers du laboratoire

Compte tenu du fait que je n'ai plus accès à mon code space, j'ai donc utilisé `jsfiddle` pour produire ma carte. J'ai procédé à la création d'un `HTml` et d'un `javascript`. Vous trouverez à la fin de ce document les codes Html et javascript utilisés pour ce travail.

Tout  d'abord, ouvrir :
```bash
https://jsfiddle.net/
```
cela devrait ressembler à ca: 

---
![alt text](<interface de base jsfiddle.png>)
---

## Étape 2: Initialisation de la carte

Dans `javascript`, on va injecter  la carte et les controleurs de carte (var map = new maplibregl.Map , var control = map.NavigationControl(...) etc...)

---
![alt text](<initialisation de la carte jsfiddle.png>)
---

## Étape 3: Ajout des  sources de données, des  couches  et des couleurs

On ajoute d'abord la fonction `map.on('load', function() {` puis on va créer les layers sous forme de variable objet pour la variable commerce , arrondissements et arrondissementslabels. appliquer ensuite la couleur avec `paint`.On ajoute aussi des `filter`
```bash
//  Ajout des sources

  map.addSource('commerces_source', {
    type: 'geojson',
    data: 'https://donnees.montreal.ca/dataset/c1d65779-d3cb-44e8-af0a-b9f2c5f7766d/resource/ece728c7-6f2d-4a51-a36d-21cd70e0ddc7/download/businesses.geojson'
  });

  map.addSource('arrondissements_source', {
    type: 'vector',
    tiles: [
      'https://donnees.montreal.ca/fr/dataset/limites-administratives-agglomeration/resource/6b313375-d9bc-4dc3-af8e-ceae3762ae6e/view/9b7beb1a-cb97-4b20-a137-65eab42a8b0f'
    ],
    minzoom: 0,
    maxzoom: 14
  });
  ```

On ajoute ensuite les couches:
```bash
 //  Ajout des couches

  map.addLayer({
    id: 'commerces',
    type: 'circle',
    source: 'commerces_source',
    paint: {
      'circle-radius': [
        'match',
        ['get', 'type'],
        'Épicerie', 5,
        'Pâtisserie/Boulangerie', 7,
        'Distributrice automatique', 4,
        'Pharmacie', 6,
        'Restaurant', 5,
        3
      ],
      'circle-color': [
        'match',
        ['get', 'type'],
        'Épicerie', 'orange',
        'Pâtisserie/Boulangerie', 'yellow',
        'Distributrice automatique', 'blue',
        'Pharmacie', 'green',
        'Restaurant', 'purple',
        'grey'
      ],
      'circle-stroke-color': '#fff',
      'circle-stroke-width': 1
    },
    filter: ['==', ['get', 'statut'], 'Ouvert']
  });

  map.addLayer({
    id: 'arrondissements',
    type: 'fill',
    source: 'arrondissements_source',
    'source-layer': 'arrondissements', // important pour vector tile
    paint: {
      'fill-outline-color': 'black',
      'fill-color': 'white',
      'fill-opacity': 0.2
    }
  });

  map.addLayer({
    id: 'arrondissement-labels',
    type: 'symbol',
    source: 'arrondissements_source',
    'source-layer': 'arrondissements', // important pour vector tile
    layout: {
      'text-field': ['get', 'nom'], 
      'text-font': ['Open Sans Bold', 'Arial Unicode MS Bold'],
      'text-size': 14,
      'text-anchor': 'center'
    },
    paint: {
      'text-color': '#111',
      'text-halo-color': '#fff',
      'text-halo-width': 1.5
    }
  });
  ```

Ensuite, on ajoute à la suite au `paint` un `filtrage` pour ne garder que ceux au statut `Ouvert`
```bash
    filter: ['==', ['get', 'statut'], 'Ouvert']
```



## Étape 4: Ajout des interactions souris

on va injecter les controleurs de souris :
Survol (mouseenter / mouseleave) : changement du curseur
Afficher une `popup` (nom + type)
Effectuer un zoom et un recentrage (flyTo)

---
![alt text](<mouse contrôle.png>)
---

## Étape 5: Résultat attendu

On fait `run` pour afficher le résultat

---
![alt text](<carte finale obtenue sur jsfiddle.net.png>)
---

---
![alt text](<carte finale avec fénêtre popup.png>)
---

---
# Codes utilisés

## Javascript

```bash
var map = new maplibregl.Map({
  container: 'map', // ID du div HTML qui contiendra la carte
  style: 'https://basemaps.cartocdn.com/gl/positron-gl-style/style.json', // fond de carte
  center: [-73.5674, 45.5019], // Montréal
  zoom: 10
});

map.on('load', function() {

  //  Ajout des sources

  map.addSource('commerces_source', {
    type: 'geojson',
    data: 'https://donnees.montreal.ca/dataset/c1d65779-d3cb-44e8-af0a-b9f2c5f7766d/resource/ece728c7-6f2d-4a51-a36d-21cd70e0ddc7/download/businesses.geojson'
  });

  map.addSource('arrondissements_source', {
    type: 'vector',
    tiles: [
      'https://donnees.montreal.ca/fr/dataset/limites-administratives-agglomeration/resource/6b313375-d9bc-4dc3-af8e-ceae3762ae6e/view/9b7beb1a-cb97-4b20-a137-65eab42a8b0f'
    ],
    minzoom: 0,
    maxzoom: 14
  });

  //  Ajout des couches

  map.addLayer({
    id: 'commerces',
    type: 'circle',
    source: 'commerces_source',
    paint: {
      'circle-radius': [
        'match',
        ['get', 'type'],
        'Épicerie', 5,
        'Pâtisserie/Boulangerie', 7,
        'Distributrice automatique', 4,
        'Pharmacie', 6,
        'Restaurant', 5,
        3
      ],
      'circle-color': [
        'match',
        ['get', 'type'],
        'Épicerie', 'orange',
        'Pâtisserie/Boulangerie', 'yellow',
        'Distributrice automatique', 'blue',
        'Pharmacie', 'green',
        'Restaurant', 'purple',
        'grey'
      ],
      'circle-stroke-color': '#fff',
      'circle-stroke-width': 1
    },
    filter: ['==', ['get', 'statut'], 'Ouvert']
  });

  map.addLayer({
    id: 'arrondissements',
    type: 'fill',
    source: 'arrondissements_source',
    'source-layer': 'arrondissements', // important pour vector tile
    paint: {
      'fill-outline-color': 'black',
      'fill-color': 'white',
      'fill-opacity': 0.2
    }
  });

  map.addLayer({
    id: 'arrondissement-labels',
    type: 'symbol',
    source: 'arrondissements_source',
    'source-layer': 'arrondissements', // important pour vector tile
    layout: {
      'text-field': ['get', 'nom'], 
      'text-font': ['Open Sans Bold', 'Arial Unicode MS Bold'],
      'text-size': 14,
      'text-anchor': 'center'
    },
    paint: {
      'text-color': '#111',
      'text-halo-color': '#fff',
      'text-halo-width': 1.5
    }
  });

  // Initialisation des contrôles de souris
  initMouseControls(map);
});

// Fonction pour initialiser les contrôles souris
function initMouseControls(map) {
  // 1. Changer le curseur lors du survol des commerces
  map.on('mouseenter', 'commerces', function() {
    map.getCanvas().style.cursor = 'pointer';
  });

  map.on('mouseleave', 'commerces', function() {
    map.getCanvas().style.cursor = '';
  });

  // 2. Clic sur un commerce : afficher une popup, zoomer et recentrer
  map.on('click', 'commerces', function(e) {
    var coordinates = e.features[0].geometry.coordinates.slice();
    var description = `<strong>${e.features[0].properties.nom}</strong><br>Type: ${e.features[0].properties.type}`;

    // Créer une popup
    new maplibregl.Popup()
      .setLngLat(coordinates)
      .setHTML(description)
      .addTo(map);

    // Zoomer et recentrer la carte
    map.flyTo({
      center: coordinates,
      zoom: 15, 
      essential: true // pour garantir que la transition
    });
  });
}
```


## Html


```bash
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Carte commerces Montréal</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link href="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.css" rel="stylesheet" />
  <style>
    body { margin: 0; padding: 0; }
    #map { position: absolute; top: 0; bottom: 0; width: 100%; }
  </style>
</head>
<body>
  <div id="map" style="width: 100%; height: 100vh;"></div>

  <script src="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.js"></script>
  <script>
    // 1. Initialiser la carte AVANT tout
    var map = new maplibregl.Map({
      container: 'map',
      style: 'https://basemaps.cartocdn.com/gl/positron-gl-style/style.json',
      center: [-73.5674, 45.5019], // Montréal
      zoom: 10
    });

    // 2. Quand la carte est chargée, on ajoute les sources et couches
    map.on('load', function() {
      map.addSource('commerces_source', {
        type: 'geojson',
        data: 'https://donnees.montreal.ca/dataset/c1d65779-d3cb-44e8-af0a-b9f2c5f7766d/resource/ece728c7-6f2d-4a51-a36d-21cd70e0ddc7/download/businesses.geojson'
      });

      map.addSource('arrondissements_source', {
        type: 'vector',
        tiles: [
          'https://donnees.montreal.ca/tiles/limites-administratives/agglomeration/{z}/{x}/{y}.pbf'
        ],
        minzoom: 0,
        maxzoom: 14
      });

      map.addLayer({
        id: 'commerces',
        type: 'circle',
        source: 'commerces_source',
        paint: {
          'circle-radius': [
            'match',
            ['get', 'type'],
            'Épicerie', 5,
            'Pâtisserie/Boulangerie', 7,
            'Distributrice automatique', 4,
            'Pharmacie', 6,
            'Restaurant', 5,
            3
          ],
          'circle-color': [
            'match',
            ['get', 'type'],
            'Épicerie', 'orange',
            'Pâtisserie/Boulangerie', 'yellow',
            'Distributrice automatique', 'blue',
            'Pharmacie', 'green',
            'Restaurant', 'purple',
            'grey'
          ],
          'circle-stroke-color': '#fff',
          'circle-stroke-width': 1
        },
        filter: ['==', ['get', 'statut'], 'Ouvert']
      });

      map.addLayer({
        id: 'arrondissements',
        type: 'fill',
        source: 'arrondissements_source',
        'source-layer': 'arrondissements', 
        paint: {
          'fill-outline-color': 'black',
          'fill-color': 'white',
          'fill-opacity': 0.2
        }
      });

      map.addLayer({
        id: 'arrondissement-labels',
        type: 'symbol',
        source: 'arrondissements_source',
        'source-layer': 'arrondissements',
        layout: {
          'text-field': ['get', 'nom'],
          'text-font': ['Open Sans Bold', 'Arial Unicode MS Bold'],
          'text-size': 14,
          'text-anchor': 'center'
        },
        paint: {
          'text-color': '#111',
          'text-halo-color': '#fff',
          'text-halo-width': 1.5
        }
      });

      // Initialiser les contrôles souris
      initMouseControls(map);
    });

    function initMouseControls(map) {
      map.on('mouseenter', 'commerces', function() {
        map.getCanvas().style.cursor = 'pointer';
      });

      map.on('mouseleave', 'commerces', function() {
        map.getCanvas().style.cursor = '';
      });

      map.on('click', 'commerces', function(e) {
        var coordinates = e.features[0].geometry.coordinates.slice();
        var description = `<strong>${e.features[0].properties.nom}</strong><br>Type: ${e.features[0].properties.type}`;
        new maplibregl.Popup()
          .setLngLat(coordinates)
          .setHTML(description)
          .addTo(map);
        map.flyTo({
          center: coordinates,
          zoom: 15,
          essential: true
        });
      });
    }
  </script>
</body>
</html>
```










  

