# Adding a Custom Image Overlay
File: [js-map-03.html](js-map-03.html) 

Now that we are going to start adding additional layers to the map, we want some way for the end user to be able to select which layers are being displayed. We do this by adding control component for layers.

You can directly add your base map layer to the layer controller when you instantiate it by passing in an object with a label, or key, (*OpenStreetMap*) and the variable (*osm*) assigned to the tilemap layer.

```javacript
const layerControl = L.control.layers({'OpenStreetMap': osm}).addTo(map);
```

Let's also add another base layer for completeness. Since we have already instatiated our layer controller, we will use the *.addBaseLayer(layer, name)* function.

```javascript
const ewi = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
    attribution: 'Tiles &copy; Esri &mdash; Source: Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community'
});

layerControl.addBaseLayer(ewi, 'ESRI World Imagery');
```

There are two types of layers that the layer controller can display; 1) base layers and 2) overlays. Base layers, like tilemaps, are mutually exclusive, meaning only one can be displayed at a time. Overlays can be any other type if layer and can be independently toggled on or off. 

We are going to add a hillshaded image as an overlay. This layer was generated in a Geographic Information System (GIS) from a digital elevation model (DEM). This hillshaded image overlay is just that, an image (i.e., JPEG or PNG). It is not an inherent spatial data format like a GeoTiFF. 

In order for the image to be displayed in the correct location, we need to manually record the corner points. In this example we are using the lower left and upper right corner of the image bounds.

- ll_x = -84.5887677
- ll_y = 9.7784494
- ur_x = -83.4716701
- ur_y = 10.7022354
- bounds = [[ll_y, ll_x], [ur_y, ur_x]]

Since our hillshade image layer is just an image, we can reference and access it directly from the web server using a relative URL. Then we can instantiate a L.imageOverlay object by passing in the URL and bound and adding it to the map as well as the layer controller. 

```javascript
const hillshade_url = '../layers/hillshade.png';
const hillshade_bounds = [[9.7784494, -84.5887677], [10.7022354, -83.4716701]];
const hillshade = L.imageOverlay(hillshade_url, hillshade_bounds).addTo(map);
layerControl.addOverlay(hillshade, 'Hillshade');
```

As mentioned earlier, the layer controller has two types of layers; 1) base layers and 2) overlays. Since our hillshaded layer is one that we may want the user to be able to toggle on and off, independently of other layers, we will call the layer controllers *addOverlay(layer, legend_name)* function.

### Note: Coordinate Systems
Most slippy maps and tilemap services consume and provide data in the "Psuedo-Mercator" or "Web-Mercator" projection. So when preparing your data, make sure you are working and exporting your data in EPSG: 3857.

Demo: [Some URL](./)

### Chapter 4: [Adding and styling a polygon layer](./Readme-04.md)