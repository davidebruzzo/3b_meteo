# Virtual Reality project - Group 3b_Meteo

This repository contains the work for the Virtual Reality project 

#### Author: Claudio Del Gaizo (S4696649), [Davide Bruzzo](mailto:davide.brzo@gmail.com?subject=[GitHub]%20Source%20Han%20Sans) (S4649449), Daniele Parisi (S)

#### mail: cdg9914@gmail.com, (mailto:davide.brzo@gmail.com)

Table of contents
----------------------

* [Introduction](#introduction)
* [Dependencies and Setup](#dependencies-and-setup)
* [Project structure](#project-structure)
* [Software Components and code description](#software-components-and-code-description)
* [Behaviuor Presentation](#behaviuor-presentation)
* [Limitations and Possible Improvements](#limitations-and-possible-improvements)


## Introduction

The goal of this assignment is to integrate the architecture developed in the first assignment with a robotic simulation., in particular we have to Add a robot to a simulation environment and Integrate (if needed, modify it) the architecture that we have developed in the first assignment to the given scenario in such a way that:
* The robot is spawned in the initial position x = -6.0, y = 11.0
* Builds the "semantic" map of the environment by detecting, without moving the base of the robot, all seven markers that are present around it, by calling the provided service node.
* Starts the patrolling algorithm by relying on autonomous navigation strategies (mapping/planning) and on the information collected and stored in the ontology during the previous step.
* When a room is reached, perform a complete scan of the room (by rotating the base or the camera).

The Map
====================================================================
The 3D environment representation of the 2D map of former assignment, which has been used for this project is shown here below:

<p><p align="center">
<img src="https://github.com/claudio-dg/assignment2/blob/main/media/GazeboMap.png?raw=true" width="500" />
<p>

	
The Robot
====================================================================
