# Restructuring the Vector Loading Process
File: [js-map-06.html](js-map-06.html) 

Up to this point we have just been adding new code. In this chapter, we are going to do a bit of refactoring to make loading the other vector layers a little easier and compact. 

Before we start refactoring our existing code, we need to add some additional containers for our other vectors. 

```javascript
map.createPane('overlayMid');
map.getPane('overlayMid').style.zIndex = 450;
map.createPane('overlayUpper');
map.getPane('overlayUpper').style.zIndex = 475;
```

By default, Leaflet has 5 main containers or panes for different map components.

- popupPane: (z-index 700) Popups. 
- markerPane: (z-index 600) Marker icons
- shadowPane: (z-index 500) Marker shadows
- overlayPane: (z-index 400) Paths like lines, polylines, circles, or GeoJSON layers
- tilePane: (z-index 200, approximately) Tilemap layers and grid layers.

The z-index is a styling property that controls the vertical stacking of overlapping elements. All of our vector layers will, by default, be in the *overlayPane*. If we were to just add other vectors, like a road, after we load our elevation the road will properly appear on top of the elevation polygons. However, if we were to toggle the elevation layer off and then on again, the elevation layer would be the top most vector layer, thus hiding any other vector layer. 

So help ensure that all of our vectors will be visible, we added two additional panes.

Next, we are going to create a simple data structure to hold some information about or vector layers that we will then pass to the loading function. 

Each entry has the following information,

- URL of the vector data
- Display name
- Styling information
- Pane into which we will render the layer

```javascript
layers = [
    ['../layers/elevation.geojson', 'Elevation', elevationStyle, 'overlayPane'],
    ['../layers/station.geojson', 'La Selva Research Station', {color: '#005500', weight: 1}, 'overlayMid'],
    ['../layers/boundary.geojson', 'Braulio Carrillo National Park', {color: '#333333', weight: 1, dashArray: '5, 5'}, 'overlayMid'],
    ['../layers/transect.geojson', 'Transect', {color: '#000000', weight: 1}, 'overlayUpper'],
    ['../layers/plots.geojson', 'Plots', {color: '#000000', weight: 1}, 'overlayUpper'],
];
```

Now that we have our custom panes and vector layer information, we are going to replace the original statement we wrote for loading the elevation polygons.

```javascript
axios.get('../layers/elevation.geojson')
.then( response => {
    const elevation = L.geoJson(response.data, {style: elevationStyle}).addTo(map);
    layerControl.addOverlay(elevation, 'Elevation');
})
.catch( error => {
    /* Here you would add error handling and notification */
    console.log(error);
})
.finally( () => {

});
```

The new function (*loadLayers*) will simply loop through the layer definitions and load them.

```javascript
const loadLayers = async (layers) => {
    for(const layerDef of layers) {
        await axios.get(layerDef[0])
        .then( response => {
            const layer = L.geoJson(response.data, {style: layerDef[2], pane: layerDef[3]}).addTo(map);
            layerControl.addOverlay(layer, layerDef[1]);
        })
        .catch( error => {
            /* Here you would add error handling and notification */
            console.log(error);
        })
        .finally( () => {

        });
    }
};
loadLayers(layers);
```
The second change we are going to make is related to the elevation legend. 

Let's remove the legend if the user hides the elevation polygons. To do this we are going to add a parameter (*show*) to the *genElevationLegend()*.

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

Lastly, we need to add some listeners that will fire when a event is triggered. Specifically, the *overlayadd* and *overlayremove* events. 

```javascript
map.on('overlayadd', function(layer){
    if (layer.name === 'Elevation'){
        genElevationLegend(true);
    } 
});

map.on('overlayremove', function(layer){
    if (layer.name === 'Elevation'){
        genElevationLegend(false);
    } 
});
```


Demo: [Some URL](./)

### Chapter 7: [Adding point data](./Readme-07.md)