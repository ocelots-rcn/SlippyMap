# Agrupar datos de puntos

Archivo: [js-map-08.html](js-map-08.html)

Demostración: https://ocelots-rcn.github.io/SlippyMap/js/es/js-map-08.html

Uno de los principales problemas de nuestro mapa es que muchos puntos se superponen. El usuario no puede observar fácilmente la distribución cuando se muestran varias especies, ni existe una indicación de cuántos puntos hay en una misma ubicación.

Una opción es agrupar los puntos. Para ello, utilizaremos el plugin MarkerCluster.

Comenzamos agregando las importaciones correspondientes en la sección de encabezado.

```html
<script>
    src="https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js"
    integrity="sha256-Hk4dIpcqOSb0hZjgyvFOP+cEmDXUKKNE/tT542ZbNQg="
    crossorigin="">
</script>

<link 
    rel="stylesheet"
    href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css"
    integrity="sha256-YU3qCpj/P06tdPBJGPax0bm6Q1wltfwjsho5TR4+TYc="
    crossorigin=""/>
<link 
    rel="stylesheet"
    href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css"
    integrity="sha256-YSWCMtmNZNwqex4CEw1nQhvFub2lmU7vcCKP+XVwwXA="
    crossorigin=""/>
```

Primero, creamos un nuevo *markerClusterGroup* y lo agregamos al mapa.

```javascript
let clusterGroup = L.markerClusterGroup({
    maxClusterRadius: 30,
    showCoverageOnHover: false}).addTo(map);
```

Luego, refactorizamos la función *displayBeetles()* para agregar los puntos al *markerClusterGroup*, en lugar de agregarlos directamente al mapa.

El *markerClusterGroup* solo puede agrupar objetos de tipo *marker* y no funciona con objetos *circleMarker*. Por lo tanto, debemos crear un ícono personalizado.

```javascript
const displayBeetles = () => {
    clusterGroup.clearLayers();
    Object.keys(species).forEach(name => {
        var customIcon = L.icon({
                iconAnchor: \[5, 5],
                iconUrl: `data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg"><circle cx="5" cy="5" r="5" fill="${species\[name].color}" /></svg>`
            });
        if(species\[name].display === true) {
            points = beetles\[species\[name].code];

            for(const point of points) {
                let marker = L.marker(\[point\[1], point\[0]], { icon: customIcon });
                clusterGroup.addLayer(marker);      
            }
        }
    });
};
```

¡Y eso es todo! Ahora contamos con un mapa interactivo que no solo muestra las ubicaciones de las especies de escarabajos, sino que también ayuda a destacar el esfuerzo de muestreo.

Demostración: [Some URL](./)

