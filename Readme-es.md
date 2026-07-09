Idioma: [Inglesa](Readme.md)
# Slippy Maps
Slippy maps, or tiled web maps, are a ubiquitous technology for creating and displaying interactive maps in a standard web browser. When used appropriately, maps can be a powerful teaching tool to promote critical thinking and spatial awareness skills, highlight relationships within data, and communicate a range of complex topics that words alone can not always convey. 

The purpose of this tutorial is to outline the fundamental steps for building a simple interactive map. For this tutorial, we will be using the popular Leaflet (https://leafletjs.com/) library. The tutorial is broken down into small, incremental code snippets that build upon each other and will cover the basics of adding base layers, adding and stylizing polygon and point layers, as well as some approaches for improving the display of highly clustered data. 

The tutorial is not and in-depth introduction to programming languages or concepts and assumes the reader has some programming background. 

## Mapping Objective
Create a map to display the distribution of beetles in the genus Cephaloleia found along an elevational transect at La Selva Research Station, Costa Rica.

## Initial Set Up
You can download this repo to your local computer by clicking the green [<> Code] button and then selecting the [download zip] option or by using
```bash
git clone https://github.com/ocelots-rcn/SlippyMap
```
if you are familiar with using the distributed version control software system git.

## Starting a Basic Web Server
To develop a slippy map, a local web server is needed as we will be loading some of the data layers dynamically. 

In the root directory of this repo there are three scripts that will help you launch a simple web server for development. 

Two of the scripts can be used if you have Python3 installed on your computer
* *server.py* -- a bash script for Linux or OSX
* *server.bat* -- a Windows batch script

If you are a R user,
* *server.R*

Once you launch one of the above scripts, you can open a browser and enter http://localhost:8080 into the address bar.

## Tutorial Outline
The primary example is written in JavaScript, the native language for web browsers. Other examples (i.e., Python and R) may be added at a later date.

* Chapter 1: Basic template ([JavaScript](./js/en/Readme-01.md))
* Chapter 2: Adding the base map ([JavaScript](./js/en/Readme-02.md))
* Chapter 3: Adding a custom image overlay ([JavaScript](./js/en/Readme-03.md))
* Chapter 4: Adding and styling a polygon layer ([JavaScript](./js/en/Readme-04.md))
* Chapter 5: Adding a legend ([JavaScript](./js/en/Readme-05.md))
* Chapter 6: Restructuring the vector loading process ([JavaScript](./js/en/Readme-06.md))
* Chapter 7: Adding point data ([JavaScript](./js/en/Readme-07.md))
* Chapter 8: Clustering point data ([JavaScript](./js/en/Readme-08.md))

## Licenses and Acknowledgements

<ins>Code</ins>

The JavaScript and Python code included in this repository are license free and released into the public domain.

<ins>Spatial Layers</ins>

The [Organization for Tropical Studies (OTS)](https://tropicalstudies.org/) provided the following spatial layers under the Creative Commons Attribution-ShareAlike 4.0 International license:

* *layers/elevation.geojson*
* *layers/hillshade.png*
* *layers/plots.geojson*
* *layers/station.geojson*
* *layers/transect.geojson*

*layers/boundary.geojson* is a subset of the Áreas Silvestres Protegidas layer available from Sistema Nacional de Áreas de Conservación (SINAC)

<ins>Beetle Data</ins>

If the *layers/beetles.json* is used for any purpose, it should carry the following citation:

García-Robledo, C., Kuprewicz, E. K., Staines, C. L., Erwin, T. L., & Kress, W. J. (2016). Limited tolerance by insects to high temperatures across tropical elevational gradients and the implications of global warming for extinction. Proceedings of the National Academy of Sciences, 113(3), 680-685.