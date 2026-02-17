# Adding and Styling a Polygon Layer
File: [js-map-04.html](js-map-04.html)

Demo: https://ocelots-rcn.github.io/SlippyMap/js/js-map-04.html

The hillshaded image overlay is a visually appealing representation of elevation, but does not provide any actual elevation information. 

To visualize the ranges in elevation, we are going to add a polygon layer that denotes 500m elevational gradients from 0m to 3500m.

All vector layers will be in the GeoJSON format and will need to be dynamically loaded rather than directly accessed like the hillshaded image was. To do this, we will need to use another library to simplify the loading of these data.

Axios is a widely used library for dynamic HTTP requests. To leverage its functionality, we need to load the library by adding the following lines to the header section of the html document.

```javascript
<!-- Extra libraries -->
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

Before we dive into loading the vector data, we need to create a helper function to stylize the polygons for visual clarity.

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
    /* Return a style for the polygon based on 'class' */
    return {
        weight: 0,
        fillOpacity: 0.75,
        fillColor: classColor
    }
};
```
The function *elevationStyle()* requires one parameter *feature* which is just a polygon in our dataset. Each polygon (feature) in our dataset has three main components; type, properties, and geometry. The *properties* hold the attributes for the polygon and as the name suggests *geometry* holds the coordinate list that defines the polygon. You can better understand the structure of the GeoJSON format by looking directly at the data layer [elevation.geojson](../layers/elevation.geojson).

The polygon's properties has an attribute called *class* which represents the elevation bin, with 1 being <= 500m, 2 being 500m to 1000m, etc.

When a feature is passed to  *elevationStyle()*, the function sets the *classColor* variable based on the polygon's class attribute (*feature.properties.class*) and returns a styling object that will be used during the loading and rendering of the polygon in the next code fragment. We set the *fillOpacity* to 0.75 in the return style object so that we can get some of the shadows from the hillshaded layer to show through the elevational gradients adding some extra visual appeal.

So let's load the elevation polygons! To do this we will leverage the Axios library.

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
Axios is a promise-based HTTP request library. JavaScript promises are complicated and their detailed description is well beyond the scope of this tutorial.

To put it simply, *axios.get(URL)* is an asynchronous HTTP GET request for some resource. It is a bit like sending an email to a colleague asking for a document. You won't get a response immediately, instead, you send the email then continue on with your day. 

There are two types of responses you will get from your colleague. If they had the resource (success) you *.then()* do something with requested document. If they did not have the document (failure) then you can *.catch()* this error and do something accordingly. Lastly, you may *.finally()* have some extra things to do regardless of whether your colleague had the requested document or not.

So, we start by making the request for the elevation.geojson file.

```javascript
axios.get('../layers/elevation.geojson')
```

Since our resource is on the same server as the rest of the code, we can just use a relative URL. However, this does not have to be the case. We can make requests to different domains / URLs for resources.

Next we are going to use method chaining to define an anonymous function that will actually process the requested if it was available.

```javascript
axios.get('../layers/elevation.geojson')
.then( response => {
    const elevation = L.geoJson(response.data, {style: elevationStyle}).addTo(map);
    layerControl.addOverlay(elevation, 'Elevation');
})
```

Leaflet has a build in class for handling GeoJSON data. So if we received the requested data we can instantiate the elevation layer by passing the data (*response.data*) and an object specifying the styling function to *L.geoJson()* and adding that new layer to the map. Like our hillshaded image, we may want the user to be able to toggle the elevation polygons on and off, independently of other layers, so we will call the layer controllers *addOverlay(layer, legend_name)* function.

To finish out the (rather long) request statement, we would also define anonymous function to define the actions in the *.catch()* and *.finally()* function, which we are going to skip in this tutorial.

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

### Note: Asynchronous Design Considerations
Asynchronous programs can get complicated. Remember, any code below what is represented in the block above will (may) be executed before the block is finished!

### Chapter 5: [Adding a legend](./Readme-05.md)