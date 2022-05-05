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
  * [Load and save project](#load-and-save-project)
- [Vector layers](#vector-layers)
  * [Load vector layer](#load-vector-layer)
  * [Edit vector layer](#edit-vector-layer)
- [Raster layers](#raster-layers)
  * [Load raster layer](#load-raster-layer)
  * [Explore raster statistics](#explore-raster-statistics)


# Opening the PyQGIS Console
**TASK: Open an empty QGIS project and save it at your preferred location. In your project folder, create a subfolder called "Data".**

In your project file (.qgz) navigate to the `Plugins > Python Console` option, which pops up the Python Console on the screen. Inside the Python Console, click the `Show Editor` button to show the editor.

*In the Console, you can run Python commands, while in the Editor, you can create .py files to save those commands or a combination of them. It works like a generic text editor and terminal combination you can find in any IDE.*

When you open the Console and Editor, there is a block of code automatically run:

```
from qgis.core import *
import qgis.utils
```
This accesses the basic functionality of Python in QGIS. You will always need this, and it will always go ahead. Additionally, you can import and install other libraries like in general Python.

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

## Load and save project

# Vector layers

## Load vector layer

## Edit vector layer

# Raster layers

## Load raster layer

## Explore raster statistics
