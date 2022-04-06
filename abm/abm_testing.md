---
title: Testing of the Agent-Based Model
---


This page explains the testing carried out on the agent-based model.

The testing was conducted using the [unittest](https://www.docs.python.org/3/library/unittest.html) framework, which is a built-in package of Python. It enables the construction of test cases that help validating the work of different units of the code. In the case of this agent-based model, [agentframework.py](https://github.com/simagyari/GEOG5990M/blob/main/agentframework.py) was tested as it contains the Agent class and the methods that give the main functionalities to the model.

Methods tested are:

| **Method** | **Functionality** |
| :----- | :------------ |
| get_x | returns the x coordinate |
| get_y | returns the y coordinate |
| set_x | changes the x coordinate |
| set_y | changes the y coordinate |
| move | randomly changes x and y coordinates |
| eat | fills the storage of agents, decreases environment values |
| str | controls printed attributes of the agent |
| sick | makes agent storage zero if it exceeds 100 |
| share_with_neighbours | shares half of storage with neighbours |
| share_eater | moves received share to storage |
| distance_between | measures distance to another agent |

The tests were mainly focussed on appropriate behaviour of the methods, with some exceptions that ensure the appropriate errors are raised when incorrect data is fed to the program. The testing code is available in the [project repository](https://github.com/simagyari/GEOG5990M) inside [tests_agentframework.py](https://github.com/simagyari/GEOG5990M/blob/main/agentframework.py).
