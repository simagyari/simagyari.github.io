---
title: Agent-Based Model
---


The purpose of this page is to showcase and describe the agent-based model for Assignment 1 of GEOG5990M Programming for Geographical Information Analysis: Core Skills.

## Description

## Development and Issues

The development of the model is following the guidelines of the practical material from Assignment 1, GEOG5990M at the University of Leeds. The code was developed between 24/01/2022 and 06/04/2022 using [Python](https://www.python.org/) 3.9.7 through the [Anaconda](https://www.anaconda.com/) distribution. The editor used was [Microsoft Visual Studio Code](https://code.visualstudio.com/) version 1.66.0 instead of the recommended [Spyder](https://www.spyder-ide.org/), since the code was made for command-line execution. VSCode provided a better environment for building the model outside of the iPython kernel, which requires different usage of [matplotlib](https://matplotlib.org/) backends and visualisation.

The main issues arising throughout the development phase were:
1. Movement of agents - to make the two-dimensional random-generated movement fair, the random range had to be divided into three parts, increasing, decrreasing, and not changing the coordinate. This way the agents are able to move in all eight direction from a cell, as well as to stay in place.
2. Speed regulation of agents - to regulate the speed of agents based on the fullness of their storage, the original idea was to create different speeds for each fullness and subsequently round them to the nearest integer to enable actual movement in the nested list of environment. However, due to time and complexity issues, the final product features a step change from 1 pixel to 2 when the storage is at least half full.
3. Iterations of the model - iterations could have been regulated through a generator function appended to the animation of matplotlib, but I deemed the manual control of the number of model runs more important than the seamless and random nature of a generator function, as there were no important events in the agent movement that could have signed the end of the simulation. Thus, the number of iterations has to be set manually, facilitating the modelling of specific time periods.

## Testing

The model was tested using the [unittest](https://docs.python.org/3/library/unittest.html) framework. For more details of the testing carried out on the project, please see [Testing](abm_testing.html).

## Profiling

The model was profiled for runtime (speed) using [cProfile](https://docs.python.org/3/library/profile.html). Memory usage was profiled with [memory-profiler](https://pypi.org/project/memory-profiler/). For details of the runtime and memory profiling carried out on the project, please see [Profiling](abm_profiling.html).
