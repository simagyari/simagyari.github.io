---
title: Tutorial 1 - PyQGIS basics
---


# Introduction
This tutorial has been made as an output of my 70-hour placement project for GEOG5230M Professional Development at MSc River Basin Dynamics and Management with GIS at the University of Leeds. The aim of this project is to showcase a possible solution for creating an e-learning tool to teach Python in QGIS to students in water-related fields.

To complete these tutorials, a basic understanding of Python, its data structures, functions, and classes is required, as well as some familiarity with the desktop usage of QGIS3. To follow the tutorial, an installation of QGIS 3.10 A Coruna Long Term Release is required. The code has not been tested and adapted to other versions of QGIS, although it might work with them.

This is the first tutorial, talking about accessing Python functionality and basic layer manipulation inside the QGIS Python Console.

# Table of Contents
- [Introduction](#introduction)
- [Opening the PyQGIS Console](#opening-the-pyqgis-console)
- [iface](#iface)
- [Project instance](#project-instance)
	* [Save project](#save-project)
- [Load layers](#load-layers)
	* [Load vector layer](#load-vector-layer)
	* [Load raster layer](#load-raster-layer)
- [Working with layers](#working-with-layers)
	* [Edit vector layer](#edit-vector-layer)
		* [Set active layer](#set-active-layer)
		* [Get features](#get-features)
		* [Editing attributes](#editing-attributes)
		* [Vector geometry](#vector-geometry)
		* [Field calculation](#field-calculation)
		* [Write vector file](#write-vector-file)
	* [Explore raster statistics](#explore-raster-statistics)
		* [Extent and resolution](#extent-and-resolution)
		* [Descriptive statistics](#descriptive-statistics)
		* [Write raster file](#write-raster-file)


# Opening the PyQGIS Console
**TASK: Open an empty QGIS project and save it at your preferred location. In your project folder, create a subfolder called "Data".**

In your project file (.qgz) navigate to the `Plugins > Python Console` option, which pops up the Python Console on the screen. Inside the Python Console, click the `Show Editor` button to show the editor.

![Image of the Toolbar route to the Console](images/t1_console.png "Open the Console from the Toolbar")

*In the Console, you can run Python commands, while in the Editor, you can create .py files to save those commands or a combination of them. It works like a generic text editor and terminal combination you can find in any IDE. You can pop up the Console using the keyboard shortcut `Ctrl+Alt+P` too.*

When you open the Console and Editor, there is a block of code automatically run:

```
from qgis.core import *
import qgis.utils
```
This accesses the basic functionality of Python in QGIS. You will always need this, and it will always go ahead. Additionally, you can import and install other libraries like in general Python.

**TASK: in the Editor, create a new .py file in the folder of your QGIS project. Name it Tutorial_1.py.**

The file will open in the Editor. The Editor's toolbar has options on running the complete code, or running the selected lines. We will use the latter frequently, as previous commands do not need reiterating to do another one in our case. When you have selected the lines you want to run, use your right mouse button and click `Run Selected Lines` for the lines to get copied into the Console. If they do not run automatically, which usually happens with multiple lines chosen, go to the Console and hit `ENTER` to run them.

![Image of the Console and the Editor](images/t1_consoleeditor.png "The Console and the Editor")

# iface
The `iface` package manages the QGIS interface, containing many useful features.

**TASK: type `help(iface)` into the Console to obtain the documentation of iface.**

# Project instance
`QgsProject.instance()` is the way to refer to the current project file. You can manipulate it with different methods of this object, such as `load()` or `write()`.

**TASK: read the folder of the project and add your data folder name to create your data directory variable.**
```
datadir = QgsProject.instance().readPath('./') + '/Data'
print(datadir)
```
*The above code reads the containing directory of the project file, then appends /Data to it.*

**TASK: get the projection of your project map, then set it to British National Grid.**
```
print(QgsProject.instance().crs())
QgsProject.instance().setCrs('EPSG:27700')
```
*EPSG:27700 is the code of the British National Grid. EPSG:4326 is that of the WGS84 ellipsoid*

## Save project
You can save your project with the command `QgsProject.instance().write(<PATH>)` from the Console if it has not been saved yet or you want to do another save to a different place. If you would like to save an already placed project (like ours now), simply use `QgsProject.instance().write()`.

**TASK: save your project with the above code.**

Notice, that the Python Editor file has not been saved. You have to use its own save button to do that.

*You can code this command at the end of your file in the editor to save all outputs surely.*

# Load layers
*Vector layers, like shapefiles, geodatabase feature classes in ArcGIS, or Geopackage feature layers in QGIS, are containing geometry information of vector layers. Raster layers, on the contrary, are images, or pixel-based representations, with possibly different bands, each of them having a value at every pixel location. This section will tell you about loading them one by one or multiple instances at once.*

## Load vector layer
Vector files can be loaded into QGIS with the command `iface.addVectorLayer(<PATH>, <LAYERNAME>, <LIBRARY>)` with the filepath of the layer. Library means the driver of the file. For example, to load a shapefile, one would usually use the driver 'ogr'.

**TASK: From your files in the Data folder, import the 'wharfe_catchment' and 'wharfe_rivers_os' shapefiles.**

*Hint: use the datadir variable you have created earlier to access the shapefiles' folder.*

![Image of loading a vector layer](images/t1_loadvector.png "Load vector layers")

## Load raster layer
Raster files can be loaded into QGIS with the command `iface.addRasterLayer(<PATH>, <LAYERNAME>)`. As all raster layers are handled by the driver 'gdal', no library specification is necessary.

**TASK: load the 'wharfe_dem.tif' raster file from the Data folder.**

![Image of loading a raster layer](images/t1_loadraster.png "Load raster layer")

## Load multiple layers at once
To load multiple layers at once, you have to define them at first as layers. You can do this with the `var = QgsVectorLayer(<PATH>, <LAYERNAME>, <LIBRARY>)` command on a vector and the `var = QgsRasterLayer(<PATH>, <LAYERNAME>)` command on a raster. This way, you assign them to an unloaded layer variable. Once you have your defined layer variables, you can use the next command:
```
QgsProject.instance().addMapLayers([LIST-OF-LAYERS])
```
*You can use this method to add one layer to the map too, just remove the 's' from the end of addMapLayers.*

The fact that you have imported your layers does not mean you can access them from the Console straight away! You have to assign them to variables each, or, which is the preferred method, list them in a dictionary.
```
layers = {}  # defining the dictionary
for layer in iface.mapCanvas().layers():
	layers[layer.name()] = layer  # each layer is accessible by name
```

*Dictionaries are unordered key-value pairs, one of the most useful data structures in Python.*
*The mapCanvas refers to the visible map of the QGIS GUI.*

# Working with layers
When it comes to working with layers, most of the options we have comes with vector representation as those files have attribute tables, we can calculate and manipulate lots of things with them. On rasters, these options do not exist, modifying their values manually is not too common either. So in this section, we are taking a look at editing vector and exploring raster layers.

## Edit vector layer
To edit a vector layer, you have to select it, which, in Console terms, usually means activation, although it is not necessary to do so, if you have your layers dictionary, where you can access the chosen layer by its name.

### Set active layer
You can get the active layer of your project using `iface.activeLayer()`, which returns to you the layer object that is active (underlined in the table of contents) currently. If you wish to change the active layer, you can use the `iface.setActiveLayer(<LAYERNAME>)` with layername being the `layers[<LAYERKEY>]` in our case for the most precision.

### Get features
The `<LAYER>.getFeatures()` command retrieves an iterable of all the features (attribute table rows) from the vector object. The iterable can be used to modify, print, retrieve, and calculate different features and values.

*You can obtain one feature too with the command `getFeature(<FEATURENUMBER>)` where the number tells the row number of the feature from top to bottom, starting with 0.*

### Editing attributes

### Vector geometry

### Field calculation

### Write vector file

## Explore raster statistics

### Extent and resolution

### Descriptive statistics

### Write raster file
