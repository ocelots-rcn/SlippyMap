Idioma: [Inglés](Readme.md)
# Mapas web interactivos
Los mapas web interactivos, o mapas web basados en tilemaps, son una tecnología ampliamente utilizada para crear y mostrar mapas interactivos en un navegador web estándar. Cuando se utilizan adecuadamente, los mapas pueden ser una poderosa herramienta didáctica para promover el pensamiento crítico y las habilidades de percepción espacial, destacar relaciones dentro de los datos y comunicar una variedad de temas complejos que no siempre pueden expresarse únicamente con palabras.

El propósito de este tutorial es describir los pasos fundamentales para construir un mapa interactivo sencillo. Para este tutorial, utilizaremos la popular biblioteca Leaflet (https://leafletjs.com/). El tutorial se divide en pequeños fragmentos de código incrementales que se construyen unos sobre otros y abarcan los conceptos básicos para agregar capas base, agregar y aplicar estilos a capas de polígonos y puntos, así como algunos métodos para mejorar la visualización de datos con un alto grado de agrupamiento.

El tutorial no es una introducción exhaustiva a los lenguajes o conceptos de programación y supone que el lector cuenta con algunos conocimientos previos de programación.

## Objetivo del mapa
Crear un mapa que muestre la distribución de los escarabajos del género *Cephaloleia* encontrados a lo largo de un transecto altitudinal en la Estación de Investigación La Selva, Costa Rica.

## Configuración inicial
Puede descargar este repositorio en su computadora haciendo clic en el botón verde [<> Code] y seleccionando la opción [download zip], o utilizando
```bash
git clone https://github.com/ocelots-rcn/SlippyMap
```
si está familiarizado con el uso de Git, un sistema distribuido de control de versiones.

## Iniciar un servidor web básico
Para desarrollar un mapa web interactivo se necesita un servidor web local, ya que algunas de las capas de datos se cargarán dinámicamente.

En el directorio raíz de este repositorio hay tres scripts que le ayudarán a iniciar un servidor web sencillo para el desarrollo.

Puede utilizar dos de los scripts si tiene Python 3 instalado en su computadora:
* *server.py* -- un script de Bash para Linux u OSX
* *server.bat* -- un script por lotes para Windows

Si utiliza R:
* *server.R*

Una vez que ejecute uno de los scripts anteriores, puede abrir un navegador e ingresar http://localhost:8080 en la barra de direcciones.

## Contenido del tutorial
El ejemplo principal está escrito en JavaScript, el lenguaje nativo de los navegadores web. En el futuro podrían agregarse otros ejemplos, por ejemplo, en Python y R.

* Capítulo 1: Plantilla básica ([JavaScript](./js/en/Readme-01.md))
* Capítulo 2: Agregar el mapa base ([JavaScript](./js/en/Readme-02.md))
* Capítulo 3: Agregar una imagen personalizada como superposición ([JavaScript](./js/en/Readme-03.md))
* Capítulo 4: Agregar y aplicar estilo a una capa de polígonos ([JavaScript](./js/en/Readme-04.md))
* Capítulo 5: Agregar una leyenda ([JavaScript](./js/en/Readme-05.md))
* Capítulo 6: Reestructurar el proceso de carga de datos vectoriales ([JavaScript](./js/en/Readme-06.md))
* Capítulo 7: Agregar datos de puntos ([JavaScript](./js/en/Readme-07.md))
* Capítulo 8: Agrupar datos de puntos ([JavaScript](./js/en/Readme-08.md))

## Licencias y agradecimientos

<ins>Código</ins>

El código JavaScript y Python incluido en este repositorio no tiene restricciones de licencia y se ha puesto a disposición del dominio público.

<ins>Capas espaciales</ins>

La [Organización para Estudios Tropicales (OET)](https://tropicalstudies.org/) proporcionó las siguientes capas espaciales bajo la licencia Creative Commons Atribución-CompartirIgual 4.0 Internacional:

* *layers/elevation.geojson*
* *layers/hillshade.png*
* *layers/plots.geojson*
* *layers/station.geojson*
* *layers/transect.geojson*

*layers/boundary.geojson* es un subconjunto de la capa Áreas Silvestres Protegidas disponible en el Sistema Nacional de Áreas de Conservación (SINAC).

<ins>Datos de escarabajos</ins>

Si se utiliza *layers/beetles.json* para cualquier propósito, deberá incluirse la siguiente cita:

García-Robledo, C., Kuprewicz, E. K., Staines, C. L., Erwin, T. L., & Kress, W. J. (2016). Limited tolerance by insects to high temperatures across tropical elevational gradients and the implications of global warming for extinction. Proceedings of the National Academy of Sciences, 113(3), 680-685.
