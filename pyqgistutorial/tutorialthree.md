---
title: Tutorial 3 - PyQGIS processing scripts
---


# Introduction
This tutorial has been made as an output of my 70-hour placement project for GEOG5230M Professional Development at MSc River Basin Dynamics and Management with GIS at the University of Leeds. The aim of this project is to showcase a possible solution for creating an e-learning tool to teach Python in QGIS to students in water-related fields.

To complete these tutorials, a basic understanding of Python, its data structures, functions, and classes is required, as well as some familiarity with the desktop usage of QGIS3. To follow the tutorial, an installation of QGIS 3.10 A Coruna Long Term Release is required. The code has not been tested and adapted to other versions of QGIS, although it might work with them.

This is the third, and final, tutorial, talking about turning Console-based processing algorithms into a Script in the Processing Toolbox with GUI functionality.

# Table of Contents
- [Create Script from Template](#create-script-from-template)
- [Setting basic properties](#setting-basic-properties)
	* [Renaming your script](#renaming-your-script)
	* [Set subgroup of your script](#set-subgroup-of-your-string)
	* [Write short helper string](#write-short-helper-string)
- [initAlgorithm](#initalgorithm)
	* [Input parameters](#input-parameters)
	* [Output parameters](#output-parameters)
- [processAlgorithm](#processalgorithm)
	* [Feedback](#feedback)
	* [Result dictionaries](#result-dictionaries)
	* [Running processing algorithms inside a script](#running-processing-algorithms-inside-a-script)
	* [Chaining processing algorithms](#chaining-processing-algorithms)
	* [Logging to the main log window](#logging-to-the-main-log-window)
	* [Running and debugging the script](#running-and-debugging-the-script)
- [Last steps](#last-steps)

# First steps
To begin the programming, we have to decide what the algorithm should do. This instance, the processing script will take a raw DEM and a catchment shapefile, then return a hydrologically correct DEM of the catchment, with flow direction, flow accumulation raster ouputs, and river nodes, segments, as well as the shape of the hydrologically correct basin itself as vector outputs.

# Create Script from Template
To turn our Tutorial 2 code into a Script, we first have to initialise one.

**TASK: From the Toolbox, open a Scripts icon, then click on `New Script from Template`.**

This will give you a file with a lot of code, not all of which you will need. We will walk through it (mostly) from top down and change, add, or remove items to make it work for our case.

![Image of Toolbox with script template opening](images/t3_toolbox.png "New Script from Template")

When looking at the window of the new script, you will see something like this:

![Image of the new script window](images/t3_newscript.png "New script template")

In the next section, we will walk through all the small, miscellaneous-looking parts of this code.

# Setting basic properties
This section is about setting the smaller methods and the main class up to display the correct name and script group of the script, as well as to display a proper helper string.

## Renaming your script
There are multiple steps to rename your script.

**TASK: rewrite the name of your main class from ExampleProcessingAlgorithm to something more meaningful (in this example, I will use HydroBasinProcessingAlgorithm).**

**TASK: write a docstring to your class.**

*Hint: something like this maybe:
    """
    This algorithm takes a DEM and a catchment shapefile,
    then computes the filled DEM, flow direction, catchment area,
    stream network, watersheds, river nodes, then selects the hydrologically
    correct catchment from the basins based on the original shapefile,
    to subsequently clip all output layers to the extent of it.
    """

**TASK: go on to the `createInstance()` method and change the return name to your algorithm name.**

**TASK: in the `name()` method, change the return value to the lowercase name of the name you want your algorithm to be called from the Toolbox. In this case, I will use hydrobasincreator.**

**TASK: in the `displayName()` method, change the return value inside `self.tr()` to the actual name you want to display. In this case, I will use HydroBasinCreator.**

*With the `tr()` method, you do not need to do anything, it works fine with any name.*

In the end of these changes, which you should do in all scripts you might write in the future, the script should look something like this:

![Image of changed script naming](images/t3_namechange.png "Changed naming")

Now that you have changed the name of your script, it is time to save it.

**TASK: save your script to the default location, something like this: `C:\Users\username\AppData\Roaming\QGIS\QGIS3\profiles\default\processing\scripts`.**

## Set subgroup of your string
Subgrouping your scripts is important to organise them. It does not affect their subfolders when saving, only their representation in the Toolbox.

**TASK: at the groups method, rewrite the return self.tr value to a subgroup name that makes sense to you. For the sake of simplicity, I will use scripts this time.**

**TASK: at the groupId method, rewrite the return value to the same value you chose in the previous task.**

This way, the toolbox will show "scripts" as a subgroup when you open your script.

![Image of the scripts subgroup](images/t3_subgroup.png "scripts subgroup")

At the end of this stage, your group part should look like this:

![Image of grouping methods](images/t3_group.png "Changed grouping")

## Write short helper string
The last one of these smaller methods is the `shortHelpString()` method. This returns a short helping string for the algorithm, which will be displayed in the GUI.

**TASK: write a short helping string for the algorithm.**

*Hint: I have written the following: Creates the hydrologically correct version of a catchment and returns catchment area, channel, junction layers.*

# initAlgorithm
This is the first large part of the code. Here we set the input and output parameters.

**TASK: remove the contents of the `initAlgorithm()` method.**

## Input parameters
The input parameters you add here, will be the ones the GUI asks from the user for the code to run. As mentioned in the beginning, the script will take two input layers:

1. Raw Digital Elevation Model
2. Catchment shapefile

Besides these, three numerical variables will be needed for the processing of the layers:

1. MINSLOPE for the Fill Sinks (wang & liu) algorithm
2. METHOD for the Catchment Area algorithm
3. THRESHOLD for the Channel Network and Drainage Basins algorithm

Overall, we need a rasterlayer (QgsProcessingParameterRasterLayer), a vector layer (QgsProcessingParameterVectorLayer) and three numbers (QgsProcessingParameterNumber) for our inputs.

**TASK: add the above three classes to the `from qgis.core import` part at the top of the file.**

With the necessary options imported, we can start writing the input parameters. The first one looks like this:
```
        # We add the input raster source. It can be any kind of raster layer
        self.addParameter(  # adds a parameter
            QgsProcessingParameterRasterLayer(  # raster layer
                self.DEM,  # referred to as the DEM constant
                self.tr('Input Digital Elevation Model'),  # text above the GUI input box
                [QgsProcessing.TypeRaster]  # type of accepted layers (only raster here)
            )
        )
```
**TASK: add the constant DEM = "DEM"  right after the constants INPUT and OUTPUT at the top of the file.**

The above framework works for every input parameter, only has to be adjusted for the types and names. Constants must be written for them, too.

**TASK: remove the constants INPUT and OUTPUT as you will not need them.**

**TASK: create the vector layer (constant is CATCHMENT) parameter by yourself.**

With the layers out of the way, it's time for the numbers now.

The first one looks like this:
```
        # Add MINSLOPE number for the Fill Sinks (wang & liu) algorithm
        self.addParameter(  # add parameter
            QgsProcessingParameterNumber(  # number
                self.MINSLOPE,  # constant for it
                self.tr('Minimum slope for filling sinks'),  # text above the input box
                type=QgsProcessingParameterNumber.Double,  # type of number
                minValue=0,  # minimum value
                defaultValue=0.01  # default value
            )
        )
```

**TASK: create the other two number parameters (constants are CATCHMENTAREAMETHOD with min and default values at 0 and max value at 5, and CHANNELTHRESHOLD with min value 1 and default value 6).**

After all these have been done, your code should look like this:

![Image of the input parameters](images/t3_inputparam.png "Input parameters")

## Output parameters

# processAlgorithm

## Feedback

## Result dictionaries

## Running processing algorithms inside a script

## Chaining processing algorithms

## Logging to the main log window

## Running and debugging the script

# Last steps
