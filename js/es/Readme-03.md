# Agregar una imagen personalizada como superposición

Archivo: [js-map-03.html](js-map-03.html)

Demostración: https://ocelots-rcn.github.io/SlippyMap/js/es/js-map-03.html

Ahora que comenzaremos a agregar capas adicionales al mapa, necesitamos una forma de que el usuario final pueda seleccionar cuáles capas se muestran. Para ello, agregamos un componente de control de capas.

Puede agregar directamente la capa base al control de capas en el momento de crearlo, pasando un objeto que contenga una etiqueta o clave (*OpenStreetMap*) y la variable (*osm*) asignada a la capa de tilemap.

```javacript
const layerControl = L.control.layers({'OpenStreetMap': osm}).addTo(map);
```

Para completar el ejemplo, agreguemos también otra capa base. Como ya hemos creado el control de capas, utilizaremos la función *.addBaseLayer(layer, name)*.

```javascript
const ewi = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World\_Imagery/MapServer/tile/{z}/{y}/{x}', {
    attribution: 'Tiles \&copy; Esri \&mdash; Source: Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community'
});

layerControl.addBaseLayer(ewi, 'ESRI World Imagery');
```

El control de capas puede mostrar dos tipos de capas: 1) capas base y 2) superposiciones. Las capas base, como los tilemaps, son mutuamente excluyentes, lo que significa que solo puede mostrarse una a la vez. Las superposiciones pueden corresponder a cualquier otro tipo de capa y pueden activarse o desactivarse de manera independiente.

Agregaremos una imagen del hillshade (sombreado de relieve) como superposición. Esta capa se generó en un Sistema de Información Geográfica (SIG) a partir de un modelo digital de elevación (MDE). Esta superposición del hillshade es simplemente una imagen, por ejemplo, un archivo JPEG o PNG. No es un formato de datos espaciales propiamente dicho, como un GeoTIFF.

Para que la imagen se muestre en la ubicación correcta, debemos registrar manualmente las coordenadas de sus esquinas. En este ejemplo utilizamos las esquinas inferior izquierda y superior derecha de los límites de la imagen.

* ll\_x = -84.5887677
* ll\_y = 9.7784494
* ur\_x = -83.4716701
* ur\_y = 10.7022354
* bounds = \[\[ll\_y, ll\_x], \[ur\_y, ur\_x]]

Como nuestra capa del hillshade es únicamente una imagen, podemos hacer referencia a ella y acceder directamente desde el servidor web mediante una URL relativa. Luego podemos crear un objeto `L.imageOverlay`, pasando la URL y los límites de la imagen, y agregarlo tanto al mapa como al control de capas.

```javascript
const hillshade\_url = '../layers/hillshade.png';
const hillshade\_bounds = \[\[9.7784494, -84.5887677], \[10.7022354, -83.4716701]];
const hillshade = L.imageOverlay(hillshade\_url, hillshade\_bounds).addTo(map);
layerControl.addOverlay(hillshade, 'Hillshade');
```

Como se mencionó anteriormente, el control de capas admite dos tipos de capas: 1) capas base y 2) superposiciones. Como queremos que el usuario pueda activar o desactivar la capa del hillshade de manera independiente de las demás capas, utilizaremos la función `addOverlay(layer, legend\_name)` del control de capas.

### Nota: sistemas de coordenadas

La mayoría de los mapas web interactivos y los servicios de tilemaps consumen y proporcionan datos en la proyección Pseudomercator o Web Mercator. Por lo tanto, al preparar los datos, asegúrese de trabajar con ellos y exportarlos en EPSG:3857.

### Capítulo 4: [Agregar y aplicar estilo a una capa de polígonos](./Readme-04.md)

