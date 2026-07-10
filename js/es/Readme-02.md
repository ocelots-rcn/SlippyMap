# Agregar el mapa base

Archivo: [js-map-02.html](js-map-02.html)

Demostración: https://ocelots-rcn.github.io/SlippyMap/js/es/js-map-02.html

El primer paso consiste en asignar una instancia de mapa a la variable global *map*. Para ello, se proporciona el identificador (*map-area*) del `div` donde se renderizará el mapa en la página y, al mismo tiempo, se establecen el centro inicial del mapa y el nivel de zoom inicial.

```html
<body>
    <div id="map-area"></div>

    <!-- JavaScript para el proyecto -->
    <script>
    var map = L.map('map-area').setView(\[10.240, -84.041], 9);
```

El encadenamiento de métodos es una práctica común en la programación orientada a objetos que permite invocar varios métodos de forma secuencial dentro de una sola instrucción. Los siguientes dos bloques de código producen el mismo resultado.

```javascript
var map = L.map('map-area').setView(\[10.240, -84.041], 9);

var map = L.map('map-area');
map.setView(\[10.240, -84.041], 9);
```

Es importante señalar que Leaflet utiliza el formato tradicional de arreglo \[latitud, longitud] para representar las coordenadas, en lugar de los esfuerzos más recientes por estandarizarlas mediante el uso consistente del orden \[x, y], que correspondería a \[longitud, latitud]. Esta convención aún varía entre bibliotecas y conjuntos de datos, por lo que es importante prestar atención al orden de las coordenadas. El segundo parámetro corresponde al nivel de zoom, cuyo rango va desde 0 hasta, por lo general, 18 o 19. La selección del nivel de zoom inicial más adecuado suele requerir un proceso de prueba y error para determinar cuál funciona mejor para el proyecto.

Ahora que contamos con un objeto de mapa, podemos asignarle una fuente de tilemap (mapa en teselas) para utilizarla como capa de mapa base. En este tutorial utilizaremos OpenStreetMap (OSM).

```javascript
const osm = L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: '\&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
    }).addTo(map);
```

Existen muchos servicios gratuitos de tilemaps que pueden utilizarse, desde OSM y mapas satelitales hasta servicios de fotografías aéreas históricas.

¡Esto es todo lo que se necesita para crear el mapa web interactivo más básico!

### Capítulo 3: [Agregar una imagen personalizada como superposición](./Readme-03.md)

