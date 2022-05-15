---
title: Industrial Site Locator
---


The purpose of this page is to showcase and describe the industrial site locator application made for Assignment 2 of GEOG5990M Programming for Geographical Information Analysis: Core Skills.

# Table of Contents
- [Description](#description)
- [Development and Issues](#development-and-issues)
- [Testing](#testing)
- [Profiling](#profiling)

# Description
This application enables the users to choose between industrial site locations on a raster map of Great Britain. The software uses three maps as input, with values between 0 and 255, depicting the geology, population, and transport characteristics of areas. These maps are then averaged based on their relative importance, which is set by the user, using three scalebars. Then it displays the map chosen by the user on a greyscale image, optionally with the top 10% of values in a blue colour scheme. The displayed maps can be saved as .csv files to any location specified by the user through a pop-up file dialog window. For technical information, and guidelines on installing and running the application, please refer to the [README](https://github.com/simagyari/GEOG5990M_Assignment2/blob/main/README.md) file of the [project repository](https://github.com/simagyari/GEOG5990M_Assignment2). To get to know more about the classes, methods, and the code itself, please refer to the [documentation](build/index.html).

![Image of application GUI](images/gui.png)

# Development and Issues
The development of this application followed the requirements laid out in the assignment description, the development took approximately 5 hours of net coding time. The Jupyter Notebook was created using [Python](https://www.python.org/) 3.9.7. through the [Anaconda](https://www.anaconda.com/) distribution and [Microsoft Visual Studio Code](https://code.visualstudio.com/) version 1.66.0 with the following extensions to facilitate the usage of Notebooks:
- Jupyter v2022.4.1021342353
- Jupyter Keymap v1.0.0
- Jupyter Notebook Renderers v1.0.6
- Gather v2022.3.0

The issues arising during development were the following:
1. Handling the matplotlib backends in the IPython kernel - To make matplotlib plot in popup windows instead of the default inline option of the Jupyter Notebook, the `%matplotlib qt` magic command is needed. This one, however, interferes with the usage of the TkAgg backend of matpltolib, needed for plotting in the tkinter windows. The issue has not been completely resolved, it only works if the cell containing the imports is only loaded once during the application run. More information on avoiding this problem can be found in the [README](https://github.com/simagyari/GEOG5990M_Assignment2/blob/main/README.md).
2. Choosing colour scheme for plotting the top 10% of suitability in blue - Since the lightness of the blue had to reflect the values, a custom colour scheme was created. The lower 90% of it is the default "gray" matplotlib colourmap, while the top is a scarcely sampled version of the bottom 90% of the "Blues_r" colourmap. This enables the user to better distinguish between gray and blue parts than using the top 10% of the blue colourmap, which would have yielded almost uniformly white values. This way the best parts are clearly delineated by the contrast of light gray and dark blue, and the highest percentages still remain high, but distinguishably from the light gray.
3. Deciding when toggling the blue display should trigger the display of the map - Since for the first display the map must be opened with the `Display map` button, not the otherwise refreshing blue checkbox, the visibility of the matplotlib Axes object had to be checked. Since no in-built method was found for this, a new object attribute was created, with a boolean value, defaulting to False. Clicking the `Display map` button sets the attribute to True, enabling the blue checkbox to trigger further displays.
4. Suppressing error message after cancelled save - Since the `asksaveasfilename` function of tkinter returns the filepath the user specifies, in the case a cancel event happens in the file dialog, the returned value is an empty string. This generates  a `FileNotFoundError` when the latter part of the code would like to start writing the current raster to the filepath. This was avoided with a try - except structure, that simply passes if the above mentioned error occurs.

# Testing

# Profiling
