# Plantilla básica

Archivo: [js-map-01.html](js-map-01.html)

Demostración: https://ocelots-rcn.github.io/SlippyMap/js/es/js-map-01.html

Este archivo representa la plantilla básica de HTML/JavaScript para crear un mapa web interactivo con Leaflet.

El bloque del encabezado contiene algunas etiquetas meta para la configuración del dispositivo, así como el código para cargar la biblioteca Leaflet.

```html
<head>
    <!-- Etiquetas meta básicas -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">

    <title>Tutorial de mapas con JavaScript</title>

    <!-- Componentes necesarios para Leaflet -->
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

    <!-- CSS del proyecto -->
    <style media="screen">
        #map-area {
        height: 500px;
        border: 2px solid #CCCCCC;
        overflow: hidden;
    }
    </style>
</head>
```

La segunda parte importante se encuentra en el cuerpo del documento, donde definimos el componente de destino (`div`) en el que finalmente se mostrará el mapa, así como el bloque de código en el que se incluirá todo el código específico de nuestro mapa web interactivo.

```html
<body>
    <div id="map-area"></div>

    <!-- JavaScript del proyecto -->
    <script>
    </script>
</body>
```

Si carga este archivo en su navegador, únicamente verá una página en blanco con un rectángulo, que es el espacio donde se mostrará el mapa.

### Capítulo 2: [Agregar el mapa base](./Readme-02.md)

