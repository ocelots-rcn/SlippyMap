# Reestructurar el proceso de carga de datos vectoriales

Archivo: [js-map-06.html](js-map-06.html)

Demostración: https://ocelots-rcn.github.io/SlippyMap/js/es/js-map-06.html

Hasta este punto, únicamente hemos agregado código nuevo. En este capítulo, realizaremos una pequeña refactorización para que la carga de las demás capas vectoriales sea más sencilla y compacta.

Antes de comenzar a refactorizar el código existente, debemos agregar algunos contenedores adicionales para las demás capas vectoriales.

```javascript
map.createPane('overlayMid');
map.getPane('overlayMid').style.zIndex = 450;
map.createPane('overlayUpper');
map.getPane('overlayUpper').style.zIndex = 475;
```

De manera predeterminada, Leaflet cuenta con cinco contenedores principales, o *panes*, para distintos componentes del mapa.

* popupPane: (z-index 700) Ventanas emergentes.
* markerPane: (z-index 600) Íconos de marcadores.
* shadowPane: (z-index 500) Sombras de los marcadores.
* overlayPane: (z-index 400) Trazados como líneas, polilíneas, círculos o capas GeoJSON.
* tilePane: (z-index 200, aproximadamente) Capas de tilemap y capas de cuadrícula.

El z-index es una propiedad de estilo que controla el orden vertical de apilamiento de los elementos superpuestos. De manera predeterminada, todas las capas vectoriales se ubicarán en *overlayPane*. Si agregáramos otras capas vectoriales, como una carretera, después de cargar la capa de elevación, la carretera aparecería correctamente sobre los polígonos de elevación. Sin embargo, si desactiváramos la capa de elevación y luego la volviéramos a activar, esta pasaría a ser la capa vectorial superior y ocultaría las demás capas vectoriales.

Para ayudar a garantizar que todas las capas vectoriales permanezcan visibles, agregamos dos *panes* adicionales.

A continuación, crearemos una estructura de datos sencilla para almacenar información sobre las capas vectoriales, la cual posteriormente pasaremos a la función de carga.

Cada entrada contiene la siguiente información:

* URL de los datos vectoriales.
* Nombre para mostrar.
* Información de estilo.
* *Pane* en el que se representará la capa.

```javascript
layers = \[
    \['../layers/elevation.geojson', 'Altitud', elevationStyle, 'overlayPane'],
    \['../layers/station.geojson', 'Estación de Investigación La Selva', {color: '#005500', weight: 1}, 'overlayMid'],
    \['../layers/boundary.geojson', 'Parque Nacional Braulio Carrillo', {color: '#333333', weight: 1, dashArray: '5, 5'}, 'overlayMid'],
    \['../layers/transect.geojson', 'Transecto', {color: '#000000', weight: 1}, 'overlayUpper'],
    \['../layers/plots.geojson', 'Parcelas', {color: '#000000', weight: 1}, 'overlayUpper'],
];
```

Ahora que contamos con nuestros *panes* personalizados y con la información de las capas vectoriales, reemplazaremos la instrucción original que escribimos para cargar los polígonos de elevación.

```javascript
axios.get('../layers/elevation.geojson')
.then( response => {
    const elevation = L.geoJson(response.data, {style: elevationStyle}).addTo(map);
    layerControl.addOverlay(elevation, 'Elevación');
})
.catch( error => {
    /\* Aquí se agregaría el manejo de errores y la notificación correspondiente \*/
    console.log(error);
})
.finally( () => {

});
```

La nueva función (*loadLayers*) simplemente recorrerá las definiciones de las capas y las cargará.

```javascript
const loadLayers = async (layers) => {
    for(const layerDef of layers) {
        await axios.get(layerDef\[0])
        .then( response => {
            const layer = L.geoJson(response.data, {style: layerDef\[2], pane: layerDef\[3]}).addTo(map);
            layerControl.addOverlay(layer, layerDef\[1]);
        })
        .catch( error => {
            /\* Aquí se agregaría el manejo de errores y la notificación correspondiente \*/
            console.log(error);
        })
        .finally( () => {

        });
    }
};
loadLayers(layers);
```

El segundo cambio que realizaremos está relacionado con la leyenda de elevación.

Eliminemos la leyenda cuando el usuario oculte los polígonos de elevación. Para ello, agregaremos un parámetro (*show*) a *genElevationLegend()*.

```javascript
const genElevationLegend = (show) => {
    if(elevationLegend !== null) {
        elevationLegend.remove();
        elevationLegend = null;
    }
    if(show === true) {
        elevationLegend = L.control({ position: "bottomleft" });
        elevationLegend.onAdd = function() {
            const div = L.DomUtil.create("div", "elevationLegend");
            ...
            return div
        }
        elevationLegend.addTo(map);
    }
};
genElevationLegend(true);
```

Por último, debemos agregar algunos *listeners* que se ejecutarán cuando se active un evento. Específicamente, los eventos *overlayadd* y *overlayremove*.

```javascript
map.on('overlayadd', function(layer){
    if (layer.name === 'Altitud'){
        genElevationLegend(true);
    } 
});

map.on('overlayremove', function(layer){
    if (layer.name === 'Altitud'){
        genElevationLegend(false);
    } 
});
```

### Capítulo 7: [Agregar datos de puntos](./Readme-07.md)

