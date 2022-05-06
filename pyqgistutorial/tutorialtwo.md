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
- [Fill Sinks and Flow Direction](#fill-sinks-and-flow-direction)
- [Catchment Area](#catchment-area)
- [Channel Network](#channel-network)
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

This tutorial uses the appropriate processing algorithms and bespoke code to take in a catchment shapefile and a Digital Elevation Model, then produce the hydrologically corrected DEM and other layers, such as river segments, nodes, catchment area, etc.

# First steps
We are going to take up the work from where we left off in the last tutorial. Open up your project with the Wharfe's raster and vector layers in it, open the Console and the Editor, which should retain your Tutorial1.py file.

**TASK: create a Tutorial2.py file in the Editor.**

**TASK: get your layers into the layers dictionary the way we learned last time.**

Now with everything reset to the previous state, we can start working on the hydrological workflow.

# Fill Sinks and Flow Direction

# Catchment Area

# Channel Network

# Channel Network and Drainage Basins

# Finding the hydrologically correct Wharfe basin

## Intersect hydrological basins with catchment shape

## Calculate area of intersected subbasins

## Obtain ID of the largest subbasin

## Create layer from the hydrologically correct Wharfe basin

## Clip all layers with the hydrologically correct Wharfe basin

# Last steps
