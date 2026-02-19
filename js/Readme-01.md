# Basic Template
File: [js-map-01.html](js-map-01.html)

Demo: https://ocelots-rcn.github.io/SlippyMap/js/js-map-01.html

This file represents the basic HTML / JavaScript template for building a slippy map with Leaflet.

The header block contains some device setup meta tags, as well as the code for loading the Leaflet library.

```html
<head>
    <!-- Base meta tags -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">

    <title>JavaScript Map Tutorial</title>

    <!-- Necessary components for Leaflet -->
    <link
        rel="stylesheet"
        href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
        integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY="
        crossorigin=""/>
    <script
        src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
        integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo="
        crossorigin="">
    </script>

    <!-- CSS for project -->
    <style media="screen">
        #map-area {
        height: 500px;
        border: 2px solid #CCCCCC;
        overflow: hidden;
    }
    </style>
</head>
```

The second important part is in the body where we define the "target" component (div) where the map will ultimately be rendered and the script block where all of our slippy map specific code will live.

```html
<body>
    <div id="map-area"></div>

    <!-- JavaScript for project -->
    <script>
    </script>
</body>
```
If you load this file into your browser, you will only see a blank page with rectangle which is where the map will be rendered.

### Chapter 2: [Adding the base map](./Readme-02.md)