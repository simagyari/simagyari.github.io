---
title: Tutorial 2 - PyQGIS processing algorithms
---


# Introduction
This tutorial has been made as an output of my 70-hour placement project for GEOG5230M Professional Development at MSc River Basin Dynamics and Management with GIS at the University of Leeds. The aim of this project is to showcase a possible solution for creating an e-learning tool to teach Python in QGIS to students in water-related fields.

To complete these tutorials, a basic understanding of Python, its data structures, functions, and classes is required, as well as some familiarity with the desktop usage of QGIS3. To follow the tutorial, an installation of QGIS 3.10 A Coruna Long Term Release is required. The code has not been tested and adapted to other versions of QGIS, although it might work with them.

This is the second tutorial, talking about processing algorithms and their usage from the PyQGIS Console.

# Table of Contents

- [Introduction](#introduction)
- [The task](#the-task)
- [First steps](#first-steps)
- [Running processing algorithms](#running-processing-algorithms)
- [Fill Sinks and Flow Direction](#fill-sinks-and-flow-direction)
- [Catchment Area](#catchment-area)
- [Channel Network and Drainage Basins](#channel-network-and-drainage-basins)
- [Finding the hydrologically correct Wharfe basin](#finding-the-hydrologically-correct-wharfe-basin)
	* [Intersect hydrological basins with catchment shape](#intersect-hydrological-basins-with-catchment-shape)
	* [Calculate area of intersected subbasins](#calculate-area-of-intersected-subbasins)
	* [Obtain ID of the largest subbasin](#obtain-id-of-the-largest-subbasin)
	* [Create layer from the hydrologically correct Wharfe basin](#create-layer-from-the-hydrologically-correct-wharfe-basin)
	* [Clip all layers with the hydrologically correct Wharfe basin](#clip-all-layers-with-the-hydrologically-correct-wharfe-basin)
- [Last steps](#last-steps)

# The task
This tutorial has been made for hydrology students, so it will showcase the necessary knowledge of PyQGIS through a water-related project. Hydraulic modelling is an extremely important part of modern hydrologists' work, as it is used in everything from modelling nutrient pollution to creating flood risk representations. However, the modelling software needs quality outputs, as otherwise the "garbage in, garbage out" principle takes effect. For this reason, inputs must be pre-processed and hydrologically corrected to create the most favourable conditions for model running.

This tutorial uses the appropriate processing algorithms and bespoke code to take in a catchment shapefile and a Digital Elevation Model, then produce the hydrologically corrected DEM and other layers, such as river segments, nodes, catchment area, etc. Our river of choice for this task is the Wharfe, which runs to the north of Leeds, with many documented flood events and important water quality issues as parts of it are designated bathing locations.

# First steps
We are going to take up the work from where we left off in the last tutorial. Open up your project with the Wharfe's raster and vector layers in it, open the Console and the Editor, which should retain your Tutorial1.py file.

**TASK: create a Tutorial2.py file in the Editor.**

**TASK: get your layers into the layers dictionary the way we learned last time.**

Now with everything reset to the previous state, we can start working on the hydrological workflow. To achieve our goals, we will use the processing algorithms of the SAGA algorithm provider which comes natively with QGIS. We will also use some of the native processing algorithms.

# Running processing algorithms
To run processing algorithms in the PyQGIS Console, you have to start with the `processing.run(<COMMAND>, <PARAMETERS>)` command. This takes the command you want to run as a script, and the parameters as a disctionary. The easiest way to obtain the right parameters is to run the tool from the GUI and then look it up in the History. History is available from the processing toolbox, clicking on the clock icon at the top.

![Image of the history object](images/t2_history.png)

This method yields us all the information we need to run the scripts. If additional information is required, the SAGA framework has a useful webpage.

*If you would like to examine more the options you have regarding processing algorithms, just run them from the GUI and look at them afterwards in the history.*

# Fill Sinks and Flow Direction
The first step is filling the sinks and creating a flow direction raster. These steps are separate in ArcGIS, but in QGIS, there is a tool called Fill Sinks (wang & liu), which does it in one step. To run the script, we must create the parameters first.
```
parameters = {
	'ELEV': wharfe_dem,
	'MINSLOPE': 0.01,
	'FILLED': 'TEMPORARY_OUTPUT',
	'FDIR': 'TEMPORARY_OUTPUT',
	'WSHED': 'TEMPORARY_OUTPUT'
	}
```
With the parameters now set, we can run the tool. We assign it to a variable to preserve the outputs.
```
fill_wangliu = processing.run('saga:fillsinkswangliu', parameters)
```

This returns a dictionary with the keys "FILLED", "FDIR", "WHSED". From this, we will only need the first two, the watersheds we will create later.

*In the processing script, the algorithm provider is stated, then a colon, then the tool.*
*There is also a Fill Sinks tool, which only creates a filled DEM.*

**TASK: add the FILLED and FDIR layers to the map.**

*Hint: you can use the QgsProject.instance().addMapLayers() command and the QgsRasterLayer() command to do this task.*

This algorithm removed the sinks from our DEM and created a flow direction raster.

![Image of Flow Direction raster](images/t2_fdir.png)

# Catchment Area
The next step is calculating the Catchment Area (or Flow Accumulation in ArcGIS and some QGIS versions). This needs the filled DEM as the "ELEVATION"  key, a "METHOD" and a "FLOW", which is the output to the same temporary output as the previous ones. The method is an integer, in this case 0, which means the D8 method, where water can flow from a cell to the eight neighbouring ones.

**TASK: create the parameters dictionary for the Catchment Area tool.**

After the parameters dict, run the tool with the following code:
```
flow_acc = processing.run('saga:catchmentarea', parameters)
```
**TASK: add the resulting "FLOW" layer to the map.**

![Image of Catchment Area raster](images/t2_flow.png)

*OPTIONAL!*
*For the human eye, this does not tell much, as the difference between numbers is too large for a detailed view. For this reason, it might be useful to calculate the 10-based logarithm of the Flow raster. This can be done with the following code:*
```
params = {'INPUT_A': flow_acc['FLOW'], 'BAND_A': 1, 'FORMULA': 'log(A)', 'OUTPUT': 'TEMPORARY_OUTPUT'}
flowacc_log = processing.run('gdal:rastercalculator', params)
```
*Adding this layer to the map, you can see much more understandable results for the human eye.

![Image of the logarithm of the Catchment Area raster](images/t2_logflow.png)

# Channel Network and Drainage Basins
Once we have the flow accumulation raster, we are able to create the channels and drainage basins/catchments. For this, we use the Channel Network and Drainage Basins tool.

# Finding the hydrologically correct Wharfe basin

## Intersect hydrological basins with catchment shape

## Calculate area of intersected subbasins

## Obtain ID of the largest subbasin

## Create layer from the hydrologically correct Wharfe basin

## Clip all layers with the hydrologically correct Wharfe basin

# Last steps
