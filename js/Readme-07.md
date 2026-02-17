# Adding Point Data
File: [js-map-07.html](js-map-07.html)

Demo: https://ocelots-rcn.github.io/SlippyMap/js/js-map-07.html

Our point data represent beetle collection locations and our overarching objective in creating this interactive map is to allow users to see their distribution and highlight the fact that different species have specific thermal tolerances which relate to elevation.

To accomplish our main objective, we know from the start that users will need to toggle on and off the different species. This interactivity is going to require a fair bit of new code.

We start by making another data structure (*species*) to hold some information about the beetle species that will be displayed. The *species* object is indexed by a key, which we will also use as the display name for layer controller. 

Each entry for a species has four attributes,

- code: the species name as it appears in the data
- color: the point color
- display: boolean value indicating if the species is being displayed
- group: a variable that holds the point layer

```javasript
let beetles = {};
const species = {
    'C. belti': {'code': 'C_belti', 'color': 'rgb(255, 0, 0)', 'display': true, group: null},
    'C. bicolor Cryptic 1': {'code': 'Ch_bicolor_Cryptic_1', 'color': 'rgb(0, 255, 0)', 'display': false, group: null},
    'C. bicolor Cryptic 2': {'code': 'Ch_bicolor_Cryptic_2', 'color': 'rgb(0, 0, 255)', 'display': false, group: null},
    'C. congener': {'code': 'C_congener', 'color': 'rgb(255, 255, 0)', 'display': false, group: null},
    'C. perplexa': {'code': 'C_perplexa', 'color': 'rgb(255, 0, 255)', 'display': false, group: null}
};
```
We need to define a function that is called when the user toggles a checkbox for each species. This is similar to the listener used to show and hide the elevation legend, but we are going to attach this callback to the checkbox itself rather than listening for an event from the map.

This function updates the *species* data object and calls *displayBeetles()* which we will define shortly.

```javascript
const toggleSpecies = (event) => {
    species[event.target.name].display = !species[event.target.name].display;
    displayBeetles();
};
```

As the beetle layers are the main attraction, we should make a dedicated layer controller. The *speciesSelection()* function follows a design pattern that is very similar to our elevation legend. Notice that we have attached our *toggleSpecies(event)* function to the *onchange* event for the checkboxes.

```javascript
const speciesSelection = () => {
    speciesControl = L.control({ position: "topright" });
    speciesControl.onAdd = function() {
        const div = L.DomUtil.create("div", "speciesControl");
        div.style['background-color'] = '#fff';
        div.style.padding = '10px';
        div.innerHTML += '<div style="font-size: 1.2em;font-weight: bold;text-align: center">Cephaloleia</div>';

        const style = 'width: 18px;height: 18px;float: left;margin-right: 8px;border: 1px solid #aaaaaa;';
        Object.keys(species).forEach(name => {
            div.innerHTML += `
                <div style="height: 24px">
                    <div style="${style}background-color: ${species[name].color}">
                        <input type="checkbox" name="${name}" onchange="toggleSpecies(event)" ${species[name].display ? 'checked=true': ''} />
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

The *displayBeetles()* function loops through each beetle species in our *species* object and adds or removes the points from the map based on the *display* attribute. 

```javascript
const displayBeetles = () => {
    Object.keys(species).forEach(name => {
        if(species[name].display === true && species[name].group === null) {
            points = beetles[species[name].code];
            markers = [];
            for(const point of points) {
                let marker = L.circleMarker([point[1], point[0]], {pane: 'markerPane', radius: 2, color: species[name].color});
                markers.push(marker);
            }
            species[name].group = L.layerGroup(markers).addTo(map);      
        }
        else if(species[name].display === false && species[name].group != null) {
            map.removeLayer(species[name].group);
            species[name].group = null;
        }
    });
};
```

And last but not least, we load the beetle data!

```javascript
axios.get('../layers/beetles.json')
.then( response => {
    beetles = response.data;
    displayBeetles();
})
.catch( error => {
    /* Here you would add error handling and notification */
    console.log(error);
})
.finally( () => {

});
```

### Chapter 7: [Clustering point data](./Readme-08.md)