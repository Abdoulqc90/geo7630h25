# Laboratoire 12

## A défaut de pouvoir utiliser `codespace`, on va travailler sur `jsfiddle`

# Créer et styliser des clusters

**1** Dans `https://jsfiddle.net/`, on va dabord initialiser la carte:

---
![alt text](<initialisation de carte.png>)
---
**2** Enuite, on va ajouter du contrôle de navigation (zoom/boussole)
`map.addControl(new maplibregl.NavigationControl());`

**3** Charger la carte

---
![alt text](<chargement de la carte.png>)
---

**4** Ajout de Couche pour les clusters

---
![alt text](<couche pour cluster.png>)
---

**5** ajout de Couche pour afficher le nombre d'éléments dans les clusters

---
![alt text](<couche pour nbre éléments dans les clusters.png>)
---

**6** ajout de Couche pour les points individuels non clusterisés

---
![alt text](<couche points individuels.png>)
---
**7** zoomer dessus Quand on clique sur un cluster, 

---
![alt text](<zoomer quand on clic sur cluster.png>)
---

**8**  afficher un popup Quand on clique sur un point non clusterisé
---
![alt text](<pop quand on clic.png>)
---

**9**  Changer le curseur de souris sur clusters et points
---
![alt text](<curseurs de souris sur les clusters.png>)
---

# Codes utilisés

## Javascript

```bash
// Initialisation de la carte
var map = new maplibregl.Map({
  container: 'map',
  style: 'https://demotiles.maplibre.org/style.json', 
  center: [-73.5674, 45.5019], // Centre sur Montréal
  zoom: 10
});

// Ajout du contrôle de navigation (zoom/boussole)
map.addControl(new maplibregl.NavigationControl());

// chargement de la carte
map.on('load', function () {
  // 1. Source GeoJSON avec clustering activé
  map.addSource('commerces_source', {
    type: 'geojson',
    data: 'https://donnees.montreal.ca/dataset/c1d65779-d3cb-44e8-af0a-b9f2c5f7766d/resource/ece728c7-6f2d-4a51-a36d-21cd70e0ddc7/download/businesses.geojson',
    cluster: true,
    clusterMaxZoom: 14,
    clusterRadius: 50
  });

  //  Couche pour les clusters
  map.addLayer({
    id: 'clusters',
    type: 'circle',
    source: 'commerces_source',
    filter: ['has', 'point_count'],
    paint: {
      'circle-color': [
        'step',
        ['get', 'point_count'],
        '#51bbd6',
        10, '#f1f075',
        30, '#f28cb1'
      ],
      'circle-radius': [
        'step',
        ['get', 'point_count'],
        15,
        10, 20,
        30, 25
      ]
    }
  });

  // Couche pour afficher le nombre d'éléments dans les clusters
  map.addLayer({
    id: 'cluster-count',
    type: 'symbol',
    source: 'commerces_source',
    filter: ['has', 'point_count'],
    layout: {
      'text-field': '{point_count_abbreviated}',
      'text-font': ['Arial Unicode MS Bold'],
      'text-size': 12
    },
    paint: {
      'text-color': '#000'
    }
  });

  //  Couche pour les points individuels non clusterisés
  map.addLayer({
    id: 'unclustered-point',
    type: 'circle',
    source: 'commerces_source',
    filter: ['!', ['has', 'point_count']],
    paint: {
      'circle-color': '#11b4da',
      'circle-radius': 5,
      'circle-stroke-width': 1,
      'circle-stroke-color': '#fff'
    }
  });

  //  Quand on clique sur un cluster, zoomer dessus
  map.on('click', 'clusters', function (e) {
    const features = map.queryRenderedFeatures(e.point, { layers: ['clusters'] });
    const clusterId = features[0].properties.cluster_id;
    map.getSource('commerces_source').getClusterExpansionZoom(clusterId, function (err, zoom) {
      if (err) return;

      map.flyTo({
        center: features[0].geometry.coordinates,
        zoom: zoom
      });
    });
  });

  // Quand on clique sur un point non clusterisé, afficher un popup
  map.on('click', 'unclustered-point', function (e) {
    const coordinates = e.features[0].geometry.coordinates.slice();
    const { nom, type } = e.features[0].properties;
    const description = `<strong>${nom}</strong><br>Type : ${type}`;

    new maplibregl.Popup()
      .setLngLat(coordinates)
      .setHTML(description)
      .addTo(map);
  });

  // Changer le curseur de souris sur clusters et points
  map.on('mouseenter', 'clusters', () => { map.getCanvas().style.cursor = 'pointer'; });
  map.on('mouseleave', 'clusters', () => { map.getCanvas().style.cursor = ''; });
  map.on('mouseenter', 'unclustered-point', () => { map.getCanvas().style.cursor = 'pointer'; });
  map.on('mouseleave', 'unclustered-point', () => { map.getCanvas().style.cursor = ''; });
});
```

---

# Html

```bash
<!DOCTYPE html>
<html lang="fr">
<head>
    <title>Clusters à Montréal</title>
    <meta charset='utf-8'>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel='stylesheet' href='https://unpkg.com/maplibre-gl@5.4.0/dist/maplibre-gl.css' />
    <script src='https://unpkg.com/maplibre-gl@5.4.0/dist/maplibre-gl.js'></script>
    <style>
        body { margin: 0; padding: 0; }
        html, body, #map { height: 100%; }
    </style>
</head>
<body>
<div id="map"></div>

<script>
    const map = new maplibregl.Map({
        container: 'map',
        style: 'https://api.maptiler.com/maps/streets/style.json?key=get_your_own_OpIi9ZULNHzrESv6T2vL',
        center: [-73.5674, 45.5019], // MONTREAL
        zoom: 11
    });

    map.on('load', () => {
        map.addSource('commerces', {
            type: 'geojson',
            data: 'https://donnees.montreal.ca/dataset/c1d65779-d3cb-44e8-af0a-b9f2c5f7766d/resource/ece728c7-6f2d-4a51-a36d-21cd70e0ddc7/download/businesses.geojson',
            cluster: true,
            clusterMaxZoom: 14,
            clusterRadius: 50
        });

        map.addLayer({
            id: 'clusters',
            type: 'circle',
            source: 'commerces',
            filter: ['has', 'point_count'],
            paint: {
                'circle-color': [
                    'step',
                    ['get', 'point_count'],
                    '#51bbd6',
                    10,
                    '#f1f075',
                    30,
                    '#f28cb1'
                ],
                'circle-radius': [
                    'step',
                    ['get', 'point_count'],
                    15,
                    10,
                    20,
                    30,
                    25
                ]
            }
        });

        map.addLayer({
            id: 'cluster-count',
            type: 'symbol',
            source: 'commerces',
            filter: ['has', 'point_count'],
            layout: {
                'text-field': '{point_count_abbreviated}',
                'text-font': ['DIN Offc Pro Medium', 'Arial Unicode MS Bold'],
                'text-size': 12
            }
        });

        map.addLayer({
            id: 'unclustered-point',
            type: 'circle',
            source: 'commerces',
            filter: ['!', ['has', 'point_count']],
            paint: {
                'circle-color': '#11b4da',
                'circle-radius': 6,
                'circle-stroke-width': 1,
                'circle-stroke-color': '#fff'
            }
        });

        map.on('click', 'clusters', async (e) => {
            const features = map.queryRenderedFeatures(e.point, {
                layers: ['clusters']
            });
            const clusterId = features[0].properties.cluster_id;
            const zoom = await map.getSource('commerces').getClusterExpansionZoom(clusterId);
            map.easeTo({
                center: features[0].geometry.coordinates,
                zoom
            });
        });

        map.on('click', 'unclustered-point', (e) => {
            const coordinates = e.features[0].geometry.coordinates.slice();
            const nom = e.features[0].properties.nom || 'Commerce';
            const type = e.features[0].properties.type || 'Type inconnu';

            new maplibregl.Popup()
                .setLngLat(coordinates)
                .setHTML(`<strong>${nom}</strong><br>Type: ${type}`)
                .addTo(map);
        });

        map.on('mouseenter', 'clusters', () => {
            map.getCanvas().style.cursor = 'pointer';
        });
        map.on('mouseleave', 'clusters', () => {
            map.getCanvas().style.cursor = '';
        });
    });
</script>
</body>
</html>
```

---
Ensuite on fait `run` pour afficher le résultat

---
![alt text](<carte avec clusters.png>)
---


# Créer et styliser une carte de chaleur (heatmap)





