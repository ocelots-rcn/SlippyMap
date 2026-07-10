# Agregar y aplicar estilo a una capa de polígonos

Archivo: [js-map-04.html](js-map-04.html)

Demostración: https://ocelots-rcn.github.io/SlippyMap/js/es/js-map-04.html

La superposición del hillshade es una representación visualmente atractiva de la elevación, pero no proporciona información numérica real sobre ella.

Para visualizar los rangos de elevación, agregaremos una capa de polígonos que representa gradientes altitudinales de 500 m, desde 0 m hasta 3500 m.

Todas las capas vectoriales estarán en formato GeoJSON y deberán cargarse dinámicamente, en lugar de accederse directamente como se hizo con el hillshade. Para ello, necesitaremos utilizar otra biblioteca que simplifique la carga de estos datos.

Axios es una biblioteca ampliamente utilizada para realizar solicitudes HTTP dinámicas. Para aprovechar su funcionalidad, debemos cargarla agregando las siguientes líneas a la sección de encabezado del documento HTML.

```html
<!-- Librerías extra -->
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

Antes de comenzar a cargar los datos vectoriales, debemos crear una función auxiliar que aplique estilo a los polígonos para facilitar su interpretación visual.

```javascript
const elevationStyle = (feature) => {
    let classColor = '';
    switch(feature.properties.class) {
        case 1:
            classColor = '#80b388';
            break;
        case 2:
            classColor = '#c6e3ad';
            break;
        case 3:
            classColor = '#f4eeb2';
            break;
        case 4:
            classColor = '#f5d485';
            break;
        case 5:
            classColor = '#f0a46a';
            break;
        case 6:
            classColor = '#da8862';
            break;
        default:
            classColor = '#FFFFFF';
    }
    /\* Devuelve un estilo para el polígono según el atributo 'class' \*/
    return {
        weight: 0,
        fillOpacity: 0.75,
        fillColor: classColor
    }
};
```

La función *elevationStyle()* requiere un parámetro, *feature*, que corresponde a un polígono del conjunto de datos. Cada polígono (*feature*) del conjunto de datos tiene tres componentes principales: *type*, *properties* y *geometry*. El componente *properties* contiene los atributos del polígono y, como su nombre lo indica, *geometry* contiene la lista de coordenadas que lo define. Puede comprender mejor la estructura del formato GeoJSON consultando directamente la capa de datos [elevation.geojson](../layers/elevation.geojson).

Las propiedades del polígono incluyen un atributo llamado *class*, que representa el intervalo de elevación: 1 corresponde a ≤ 500 m; 2, al intervalo de 500 m a 1000 m; y así sucesivamente.

Cuando se pasa un elemento a *elevationStyle()*, la función establece la variable *classColor* según el atributo de clase del polígono (*feature.properties.class*) y devuelve un objeto de estilo que se utilizará al cargar y representar el polígono en el siguiente fragmento de código. En el objeto de estilo devuelto, establecemos *fillOpacity* en 0.75 para permitir que parte de las sombras de la capa del hillshade se vean a través de los gradientes altitudinales, lo que mejora su apariencia visual.

Ahora carguemos los polígonos de elevación. Para ello, utilizaremos la biblioteca Axios.

```javascript
axios.get('../layers/elevation.geojson')
.then( response => {
    const elevation = L.geoJson(response.data, {style: elevationStyle}).addTo(map);
    layerControl.addOverlay(elevation, 'Elevation');
})
.catch( error => {
    /\* Aquí se agregaría el manejo de errores y la notificación correspondiente \*/
    console.log(error);
})
.finally( () => {

});
```

Axios es una biblioteca para realizar solicitudes HTTP basada en promesas. Las promesas de JavaScript pueden ser complejas y explicarlas en detalle está fuera del alcance de este tutorial.

En términos sencillos, *axios.get(URL)* es una solicitud HTTP GET asíncrona para obtener un recurso. Es similar a enviar un correo electrónico a un colega para pedirle un documento. No recibirá una respuesta de inmediato; en su lugar, enviará el correo y continuará con sus demás actividades.

Puede recibir dos tipos de respuesta de su colega. Si tiene el recurso solicitado, es decir, si la solicitud tiene éxito, usted puede utilizar *.then()* para hacer algo con el documento recibido. Si no tiene el documento, es decir, si la solicitud falla, puede utilizar *.catch()* para capturar el error y actuar en consecuencia. Por último, puede utilizar *.finally()* para ejecutar acciones adicionales, independientemente de que su colega tuviera o no el documento solicitado.

Comenzamos realizando la solicitud del archivo `elevation.geojson`.

```javascript
axios.get('../layers/elevation.geojson')
```

Como el recurso se encuentra en el mismo servidor que el resto del código, podemos utilizar una URL relativa. Sin embargo, esto no es obligatorio: también podemos realizar solicitudes a otros dominios o URL para obtener recursos.

A continuación, utilizaremos el encadenamiento de métodos para definir una función anónima que procesará el recurso solicitado si está disponible.

```javascript
axios.get('../layers/elevation.geojson')
.then( response => {
    const elevation = L.geoJson(response.data, {style: elevationStyle}).addTo(map);
    layerControl.addOverlay(elevation, 'Elevation');
})
```

Leaflet cuenta con una clase integrada para manejar datos GeoJSON. Por lo tanto, si recibimos los datos solicitados, podemos crear la capa de elevación pasando los datos (*response.data*) y un objeto que especifica la función de estilo a *L.geoJson()*, y luego agregar esa nueva capa al mapa. Al igual que con la imagen del hillshade, es posible que queramos permitir que el usuario active o desactive los polígonos de elevación de manera independiente de las demás capas. Para ello, utilizaremos la función *addOverlay(layer, legend\_name)* del control de capas.

Para completar esta solicitud, que es bastante extensa, también podríamos definir funciones anónimas para establecer las acciones de *.catch()* y *.finally()*. Sin embargo, omitiremos esos detalles en este tutorial.

```javascript
axios.get('../layers/elevation.geojson')
.then( response => {
    const elevation = L.geoJson(response.data, {style: elevationStyle}).addTo(map);
    layerControl.addOverlay(elevation, 'Elevation');
})
.catch( error => {
    /\* Aquí se agregaría el manejo de errores y la notificación correspondiente \*/
    console.log(error);
})
.finally( () => {

});
```

### Nota: consideraciones sobre el diseño asíncrono

Los programas asíncronos pueden volverse complejos. Recuerde que cualquier código ubicado después del bloque anterior puede ejecutarse antes de que dicho bloque haya finalizado.

### Capítulo 5: [Agregar una leyenda](./Readme-05.md)

