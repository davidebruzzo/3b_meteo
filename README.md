# Virtual Reality project - Group 3b_Meteo

This repository contains the work for the Virtual Reality project 

#### Author: Claudio Del Gaizo (S4696649), [Davide Bruzzo](mailto:davide.brzo@gmail.com?subject=[GitHub]%20Source%20Han%20Sans) (S4649449), Daniele Parisi (S)

#### mail: cdg9914@gmail.com, davide.brzo@gmail.com

Table of contents
----------------------

* [Introduction](#introduction)
* [Environment](#environment)
* [Drone](#drone)
* [Crowd](#crowd)
* [Criminal person](#criminal-person)

## Introduction

This readme contains the description of the main components of the project.

The aim of the simulation is to replicate a police patrolling drone operating in narrow streets, in particular Genoa Historical Center. 

Down below you can find specific and detailed description of the code we developed to obtain the behaviour of the patrolling drone, randomic crowd walking and suspicious criminal behaviour, as well as few lines on the environment we built. 

	
## Environment

To recreate the narrow streets typical of Genoa Center we used different 3d models for buildings, pubs, church and the floor. The first map that is showed when you launch the simulation is built on the combination of these 3d assets.
Here the list of the folder that contain parts of the environment:
* [bench_AND_fence](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/bench_AND_fence?csf=1&web=1&e=EFo7UT): Model of the bench and fence that are used into playground. The fence in particular is placed also along the path for the church.
* [fountain](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/fountain?csf=1&web=1&e=u6gfKg): Model of the fountain placed in the middle of the square.
* [guns](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/guns?csf=1&web=1&e=vczd6Z): Model of the gun (Tanfoglio) which is handled by the criminal guy.
* [kids_park](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/kids_park?csf=1&web=1&e=KVdXnr): This folder contains the park games (girandola, slide etc.) which are into the playground.
* [main_Building](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/main_Building?csf=1&web=1&e=R9nZCM): Here there is the asset of the main building.
* [other_building](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/main_Building?csf=1&web=1&e=R9nZCM): All the other buildings like shops, vending machine place etc. are contained here.

## Drone
First of all we took the model of a drone from internet and put all the related files in the "[Drone](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/Drone?csf=1&web=1&e=MM5g04)" folder; moreover we created our own [animations](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/Drone/Animations?csf=1&web=1&e=zpTkty) to make it move in a more realistic way, that is by moving the propellers and slightly bending tha chassis.

Our Drone has 3 main components that describe its behaviour: first it moves along the map through a pre-defined patrolling path, then during such motion it scans the environment through a camera to detect potential dangerous situations; in the end when a criminal is detected, the drone keeps track of such character maintaining it within its own field of view and alerting other citizens. The code that implements these different behaviours is all contained in the Blueprint of the Drone ([Drone_BP](https://unigeit.sharepoint.com/:u:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/Drone/Drone_BP.uasset?csf=1&web=1&e=8zeaxI))

### 1) Patrolling path
For moving the drone we -----usato frecce per creare path predef da seguire....spiega codice :funzione usata (quale e perchè) e come abbiamo ottenuto movimento smooth tramite settare il delay e parametri come alpha

### 2) Environment Perception
per detectare pericoli abbiamo usato AI perception component settando sight come senso chiave...ogni volta che un nuovo cristiano viene detectato viene tentato un cast al Bluepirnt del delinquente..in questo modo quindi la detection è specifica di quel cristiano e non effettua un reale image recognitio/processing....spiega codice 

### 3) Character Chasing

	
## Crowd


## Criminal Person

### The Map
====================================================================
The 3D environment representation of the 2D map of former assignment, which has been used for this project is shown here below:

<p><p align="center">
<img src="https://github.com/claudio-dg/assignment2/blob/main/media/GazeboMap.png?raw=true" width="500" />
<p>
