# Agregar datos de puntos

Archivo: [js-map-07.html](js-map-07.html)

Demostración: https://ocelots-rcn.github.io/SlippyMap/js/es/js-map-07.html

Nuestros datos de puntos representan sitios de recolección de escarabajos. El objetivo general de este mapa interactivo es permitir que los usuarios observen su distribución y destacar que las distintas especies tienen tolerancias térmicas específicas relacionadas con la elevación.

Para alcanzar este objetivo principal, sabemos desde el inicio que los usuarios necesitarán activar y desactivar las distintas especies. Esta interactividad requerirá una cantidad considerable de código nuevo.

Comenzamos creando otra estructura de datos (*species*) para almacenar información sobre las especies de escarabajos que se mostrarán. El objeto *species* está indexado mediante una clave, que también utilizaremos como nombre para mostrar en el control de capas.

Cada entrada correspondiente a una especie tiene cuatro atributos:

* code: nombre de la especie tal como aparece en los datos.
* color: color de los puntos.
* display: valor booleano que indica si la especie se está mostrando.
* group: variable que almacena la capa de puntos.

```javascript
let beetles = {};
const species = {
    'C. belti': {'code': 'C\\\_belti', 'color': 'rgb(255, 0, 0)', 'display': true, group: null},
    'C. bicolor Cryptic 1': {'code': 'Ch\\\_bicolor\\\_Cryptic\\\_1', 'color': 'rgb(0, 255, 0)', 'display': false, group: null},
    'C. bicolor Cryptic 2': {'code': 'Ch\\\_bicolor\\\_Cryptic\\\_2', 'color': 'rgb(0, 0, 255)', 'display': false, group: null},
    'C. congener': {'code': 'C\\\_congener', 'color': 'rgb(255, 255, 0)', 'display': false, group: null},
    'C. perplexa': {'code': 'C\\\_perplexa', 'color': 'rgb(255, 0, 255)', 'display': false, group: null}
};
```

Debemos definir una función que se ejecute cuando el usuario active o desactive la casilla de verificación correspondiente a cada especie. Esto es similar al *listener* utilizado para mostrar y ocultar la leyenda de elevación, pero en este caso asociaremos este *callback* directamente con la casilla de verificación, en lugar de escuchar un evento proveniente del mapa.

Esta función actualiza el objeto de datos *species* y llama a *displayBeetles()*, que definiremos más adelante.

```javascript
const toggleSpecies = (event) => {
    species\\\[event.target.name].display = !species\\\[event.target.name].display;
    displayBeetles();
};
```

Como las capas de escarabajos son el elemento principal del mapa, conviene crear un control de capas específico para ellas. La función *speciesSelection()* sigue un patrón de diseño muy similar al de la leyenda de elevación. Observe que asociamos la función *toggleSpecies(event)* con el evento *onchange* de las casillas de verificación.

```javascript
const speciesSelection = () => {
    speciesControl = L.control({ position: "topright" });
    speciesControl.onAdd = function() {
        const div = L.DomUtil.create("div", "speciesControl");
        div.style\\\['background-color'] = '#fff';
        div.style.padding = '10px';
        div.innerHTML += '<div style="font-size: 1.2em;font-weight: bold;text-align: center">Cephaloleia</div>';

        const style = 'width: 18px;height: 18px;float: left;margin-right: 8px;border: 1px solid #aaaaaa;';
        Object.keys(species).forEach(name => {
            div.innerHTML += `
                <div style="height: 24px">
                    <div style="${style}background-color: ${species\\\[name].color}">
                        <input type="checkbox" name="${name}" onchange="toggleSpecies(event)" ${species\\\[name].display ? 'checked=true': ''} />
                    </div>
                    ${name}
                </div>
            `;
        });
        
        return div
    }
    speciesControl.addTo(map);
    L.DomEvent.disableClickPropagation(speciesControl.getContainer());
};
speciesSelection();
```

La función *displayBeetles()* recorre cada una de las especies de escarabajos incluidas en el objeto *species* y agrega o elimina sus puntos del mapa según el atributo *display*.

```javascript
const displayBeetles = () => {
    Object.keys(species).forEach(name => {
        if(species\\\[name].display === true \\\&\\\& species\\\[name].group === null) {
            points = beetles\\\[species\\\[name].code];
            markers = \\\[];
            for(const point of points) {
                let marker = L.circleMarker(\\\[point\\\[1], point\\\[0]], {pane: 'markerPane', radius: 2, color: species\\\[name].color});
                markers.push(marker);
            }
            species\\\[name].group = L.layerGroup(markers).addTo(map);      
        }
        else if(species\\\[name].display === false \\\&\\\& species\\\[name].group != null) {
            map.removeLayer(species\\\[name].group);
            species\\\[name].group = null;
        }
    });
};
```

Por último, cargamos los datos de los escarabajos.

```javascript
axios.get('../layers/beetles.json')
.then( response => {
    beetles = response.data;
    displayBeetles();
})
.catch( error => {
    /\\\* Aquí se agregaría el manejo de errores y la notificación correspondiente \\\*/
    console.log(error);
})
.finally( () => {

});
```

### Capítulo 8: [Agrupar datos de puntos](./Readme-08.md)

