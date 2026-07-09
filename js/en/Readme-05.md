# Adding a Legend
File: [js-map-05.html](js-map-05.html)

Demo: https://ocelots-rcn.github.io/SlippyMap/js/js-map-05.html

While the hillshaded layer and the colored elevational polygons are visually appealing and help to illustrate the elevation gradients, they still don't actually inform the end user of the elevations. So let's add a legend to the map!

Our design pattern will be as follows,

- Initialize a variable (*elevationLegend*) to store the legend object
- Create a function (*genElevationLegend()*) to create the legend
- Call the *genElevationLegend()* to actually add the legend to the map

```javascript
let elevationLegend = null;
const genElevationLegend = () => {
    if(elevationLegend !== null) {
        elevationLegend.remove();
    }
    elevationLegend = L.control({ position: "bottomleft" });
    elevationLegend.onAdd = function() {
        const div = L.DomUtil.create("div", "elevationLegend");
        div.style['background-color'] = '#fff';
        div.style.padding = '10px';
        div.innerHTML += '<span style="font-size: 1.2em;font-weight: bold">Elevation (m)</span><br/>';

        /* Add entry for each elevation bin */
        const style = 'width: 18px;height: 18px;float: left;margin-right: 8px;border: 1px solid #aaaaaa;';
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #80b388"></i> &lt;= 500</div>`;
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #c6e3ad"></i>500 &ndash; 1000</div>`;
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #f4eeb2"></i>1000 &ndash; 1500</div>`;
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #f5d485"></i>1500 &ndash; 2000</div>`;
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #f0a46a"></i>2000 &ndash; 2500</div>`;
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #da8862"></i>2500 &ndash; 3000</div>`;
        div.innerHTML += `<div style="height: 24px"><i style="${style}background-color: #ffffff"></i>3000 &ndash; 3500</div>`;
        return div
    }
    elevationLegend.addTo(map);
};
genElevationLegend();
```

**Why define a function to build the legend and then immediately call that function?**

At this point in the tutorial, it is not necessary. However, it is a convienient design pattern to implement from the beginning as your legend(s) may need to be dynamic and change as layers are toggled on and off or the data are filtered or subset by the user through other interface controls. 

Let's break down the legend generation function into its main logic sections. When the *genElevationLegend()* is called:

- If the legend already exists, remove it from the map
- Instantiate a new control (legend) object
- Override the default *L.control.onAdd()* to actually build the legend
- Add the newly created legend to the map

### Chapter 6: [Restructuring the vector loading process](./Readme-06.md)