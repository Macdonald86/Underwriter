<!DOCTYPE html>
<html>
<head>
  <title>My Farm Monitor</title>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/leaflet-draw@1.0.4/dist/leaflet.draw.css"/>

  <style>
    body { margin:0; padding:0; }
    #map { height: 400px; }
    canvas { width: 100% !important; max-width: 800px; }
  </style>
</head>
<body>

<h2>🗺️ Draw your farm field:</h2>
<div id="map"></div>

<h2>🌿 NDVI Chart:</h2>
<canvas id="ndviChart"></canvas>

<script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>
<script src="https://cdn.jsdelivr.net/npm/leaflet-draw@1.0.4/dist/leaflet.draw.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<script>
  const map = L.map('map').setView([-17.82, 31.05], 8); // Harare center
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

  const drawnItems = new L.FeatureGroup();
  map.addLayer(drawnItems);

  const drawControl = new L.Control.Draw({
    edit: { featureGroup: drawnItems }
  });
  map.addControl(drawControl);

  map.on(L.Draw.Event.CREATED, function (e) {
    drawnItems.clearLayers(); // remove previous drawing
    drawnItems.addLayer(e.layer);

    const coords = e.layer.toGeoJSON().geometry.coordinates[0];
    console.log("User field coordinates:", coords);

    alert("Now copy the coordinates from the browser console and paste them into your Google Earth Engine code!");
  });

  // Dummy NDVI chart (you'll replace it with real one from GEE)
  const ctx = document.getElementById('ndviChart').getContext('2d');
  const ndviChart = new Chart(ctx, {
    type: 'line',
    data: {
      labels: ['Jan', 'Feb', 'Mar', 'Apr'],
      datasets: [{
        label: 'NDVI Value',
        data: [0.3, 0.45, 0.55, 0.6],
        borderColor: 'green',
        fill: false
      }]
    },
    options: {
      scales: {
        y: { min: 0, max: 1 }
      }
    }
  });
</script>

</body>
</html>
