# Adding the Base Map
File: [js-map-02.html](js-map-02.html)

Demo: https://ocelots-rcn.github.io/SlippyMap/js/js-map-02.html

The first step is to assign an map instance to the global variable *map* by passing in the id (*map-area*) of the div where the map will be rendered on the page and at the same time setting the origin of the map and the initial zoom level.
```html
<body>
    <div id="map-area"></div>

    <!-- JavaScript for project -->
    <script>
    var map = L.map('map-area').setView([10.240, -84.041], 9);
```

Method chaining is a common approach in object oriented programming to invoke multiple methods sequentially in a single statement. The following two code blocks would provide the same result.

```javascript
var map = L.map('map-area').setView([10.240, -84.041], 9);

var map = L.map('map-area');
map.setView([10.240, -84.041], 9);
```

It is important to note that Leaflet uses the older [latitude, longitude] array format for coordinates rather than the recent efforts standardize the coordinate consistently by always using [x, y] which would be [longitude, latitude]. This still varies between libraries and datasets so it is important to be vigilant. The second parameter is the zoom level which ranges from 0 to typically 18 or 19. The best initial zoom level is often just a matter of trail and error to find the most suitable initial zoom level for your project.

Now that we have a map object, we can assign a tilemap source to be our base map layer. For this tutorial we are using OpenStreetMap (OSM).

```javascript
const osm = L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
    }).addTo(map);
```
There are many different free tilemap services that can be used, from OSM to satellite to historical aerial photo services.

That is all that is required to have the most basic interactive slippy map!

### Chapter 3: [Adding a custom image overlay](./Readme-03.md)