# Agregar una leyenda

Archivo: [js-map-05.html](js-map-05.html)

Demostración: https://ocelots-rcn.github.io/SlippyMap/js/es/js-map-05.html

Aunque la capa del hillshade y los polígonos de elevación coloreados son visualmente atractivos y ayudan a representar los gradientes altitudinales, todavía no informan al usuario final sobre los valores de elevación. Por lo tanto, agreguemos una leyenda al mapa.

El patrón de diseño será el siguiente:

* Inicializar una variable (*elevationLegend*) para almacenar el objeto de la leyenda.
* Crear una función (*genElevationLegend()*) que genere la leyenda.
* Llamar a *genElevationLegend()* para agregar la leyenda al mapa.

```javascript
let elevationLegend = null;
const genElevationLegend = () => {
    if(elevationLegend !== null) {
        elevationLegend.remove();
    }
    elevationLegend = L.control({ position: "bottomleft" });
    elevationLegend.onAdd = function() {
        const div = L.DomUtil.create("div", "elevationLegend");
        div.style\['background-color'] = '#fff';
        div.style.padding = '10px';
        div.innerHTML += '<span style="font-size: 1.2em;font-weight: bold">Altitud (m s. n. m.)</span><br/>';

        /\* Agregar una entrada para cada intervalo de elevación \*/
        const style = 'width: 18px;height: 18px;float: left;margin-right: 8px;border: 1px solid #aaaaaa;';
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #80b388"></i> \&lt;= 500</div>`;
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #c6e3ad"></i>500 \&ndash; 1000</div>`;
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #f4eeb2"></i>1000 \&ndash; 1500</div>`;
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #f5d485"></i>1500 \&ndash; 2000</div>`;
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #f0a46a"></i>2000 \&ndash; 2500</div>`;
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #da8862"></i>2500 \&ndash; 3000</div>`;
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #ffffff"></i>3000 \&ndash; 3500</div>`;
        return div
    }
    elevationLegend.addTo(map);
};
genElevationLegend();
```

**¿Por qué definir una función para construir la leyenda y llamarla inmediatamente después?**

En este punto del tutorial no es estrictamente necesario. Sin embargo, es un patrón de diseño conveniente para implementar desde el inicio, ya que las leyendas pueden necesitar ser dinámicas y cambiar cuando las capas se activan o desactivan, o cuando el usuario filtra o selecciona subconjuntos de los datos mediante otros controles de la interfaz.

Descompongamos la función que genera la leyenda en sus principales secciones lógicas. Cuando se llama a *genElevationLegend()*:

* Si la leyenda ya existe, se elimina del mapa.
* Se crea una nueva instancia de un objeto de control, que funcionará como leyenda.
* Se sobrescribe el comportamiento predeterminado de *L.control.onAdd()* para construir la leyenda.
* Se agrega la leyenda recién creada al mapa.

### Capítulo 6: [Reestructurar el proceso de carga de datos vectoriales](./Readme-06.md)

