# Clustering Point Data
File: [js-map-08.html](js-map-08.html)

Demo: https://ocelots-rcn.github.io/SlippyMap/js/js-map-08.html

A major issue with our map is that many of the points overlap. The user can't easily see the distribution when multiple species are displayed and there is no indication of how many points there are at any one location.

One thing we can do is to cluster the points. To do this, we will leverage the marker cluster plugin. 

We start by adding the imports to the header section.  

```javascript
script 
    src="https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js"
    integrity="sha256-Hk4dIpcqOSb0hZjgyvFOP+cEmDXUKKNE/tT542ZbNQg="
    crossorigin="">
</script>

<link 
    rel="stylesheet"
    href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css"
    integrity="sha256-YU3qCpj/P06tdPBJGPax0bm6Q1wltfwjsho5TR4+TYc="
    crossorigin=""/>
<link 
    rel="stylesheet"
    href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css"
    integrity="sha256-YSWCMtmNZNwqex4CEw1nQhvFub2lmU7vcCKP+XVwwXA="
    crossorigin=""/>
```

First, we create a new *markerClusterGroup* and add it to the map.

```javascript
let clusterGroup = L.markerClusterGroup({
    maxClusterRadius: 30,
    showCoverageOnHover: false}).addTo(map);
```

Then we refactor our *displayBeetles()* function to add the points to the *markerClusterGoup* rather than adding the points directly to the map. 

The *markerClusterGroup* can only cluster *marker* objects and will not work with *circleMarker* objects. So, we have to make a custom icon.

```javascript
const displayBeetles = () => {
    clusterGroup.clearLayers();
    Object.keys(species).forEach(name => {
        var customIcon = L.icon({
                iconAnchor: [5, 5],
                iconUrl: `data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg"><circle cx="5" cy="5" r="5" fill="${species[name].color}" /></svg>`
            });
        if(species[name].display === true) {
            points = beetles[species[name].code];

            for(const point of points) {
                let marker = L.marker([point[1], point[0]], { icon: customIcon });
                clusterGroup.addLayer(marker);      
            }
        }
    });
};
```

And that is it! We now have an interactive map that shows not only the locations of the beetle species but also helps to highlight the sampling effort. 

Demo: [Some URL](./)