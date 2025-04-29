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









  

