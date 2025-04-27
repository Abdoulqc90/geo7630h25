# Lab 11 – Cartographie interactive avec MapLibreGL

## Étape 1: création de fichiers du laboratoire

Dans le code space, on va créer des fichiers dans le dossier laboratoire (dans semaine 11):
`index.html` : point d’entrée HTML
`map-controls.j` : création et configuration de la carte
`map-layers.js` : définition des sources et couches
`app.js` : chargement dynamique des couches dans la carte
`mouse-controls.js` : interactions avec la souris (hover, click)

---
![alt text](<création de fichiers.png>)
---

## Étape 2: Initialisation de la carte

Dans `map-controls.js`, on va injecter  la carte et les controleurs de carte (var map = new maplibregl.Map , var control = map.NavigationControl(...) etc...)

---
![alt text](<initialisation de la carte.png>)
---

## Étape 3: Ajout des couches de données

Dans le fichier `map-layers.js`, on va créer les layers sous forme de variable objet pour la variable commerce :
```bash
// Définition de la source GeoJSON
var commercesSource = {
    type: 'geojson',
    data: 'https://donnees.montreal.ca/dataset/c1d65779-d3cb-44e8-af0a-b9f2c5f7766d/resource/ece728c7-6f2d-4a51-a36d-21cd70e0ddc7/download/businesses.geojson'
  };
  
  // Définition de la couche avec symbologie par type de commerce
  var commercesLayer = {
    id: 'commerces',
    type: 'circle',
    source: 'commerces_source'
    
  };
  ```
  Puis on ajoute la couleur et la taille variables selon le type de commerce:
```bash
 paint: {
      // Rayon variable selon le type
      'circle-radius': [
        'match',
        ['get', 'type'],
        'Épicerie', 5,
        'Pâtisserie/Boulangerie', 7,
        'Distributrice automatique', 4,
        'Pharmacie', 6,
        'Restaurant', 5,
        3 // taille par défaut
      ],
      // Couleur variable selon le type
      'circle-color': [
        'match',
        ['get', 'type'],
        'Épicerie', 'orange',
        'Pâtisserie/Boulangerie', 'yellow',
        'Distributrice automatique', 'blue',
        'Pharmacie', 'green',
        'Restaurant', 'purple',
        'grey' // couleur par défaut
      ],
      'circle-stroke-color': '#fff',
      'circle-stroke-width': 1
    }
```
Ensuite, on ajoute à la suite du `paint` un `filtrage` pour ne garder que ceux au statut `Ouvert`
```bash
    filter: ['==', ['get', 'statut'], 'Ouvert']
```

## Étape 4: Chargement des couches dans la carte

Dans `app.js`, on va injecter les layers précédement créer dans le `map-layers.js`


## Étape 5: Ajout des interactions souris

Dans `mouse-controls.js`, on va injecter les controleurs de souris :
Survol (mouseenter / mouseleave) : changement du curseur
Afficher une `popup` (nom + type)
Effectuer un zoom et un recentrage (flyTo)


## Étape 6: Extension possible




  

