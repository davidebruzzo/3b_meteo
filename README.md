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

To recreate the narrow streets typical of Genoa Center we used different 3d models for buildings, pubs, church and the floor, using predefined assets or creating custom ones on our own. The first map that is showed when you launch the simulation is built on the combination of these 3d assets.
Here the list of the folder that contain parts of the environment:
* [bench_AND_fence](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/bench_AND_fence?csf=1&web=1&e=EFo7UT): Model of the bench and fence that are used into playground. The fence in particular is placed also along the path for the church.
* [fountain](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/fountain?csf=1&web=1&e=u6gfKg): Model of the fountain placed in the middle of the square.
* [guns](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/guns?csf=1&web=1&e=vczd6Z): Model of the gun (Tanfoglio) which is handled by the criminal guy.
* [kids_park](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/kids_park?csf=1&web=1&e=KVdXnr): This folder contains the park games (girandola, slide etc.) which are into the playground.
* [main_Building](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/main_Building?csf=1&web=1&e=R9nZCM): Here there is the asset of the main building.
* [other_building](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/main_Building?csf=1&web=1&e=R9nZCM): All the other buildings like shops, vending machine place etc. are contained here.

## Drone
First of all we took the model of a drone from internet and put all the related files in the "[Drone](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/Drone?csf=1&web=1&e=MM5g04)" folder.

For such drone we created our own [animations](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/Drone/Animations?csf=1&web=1&e=zpTkty) to make it move in a more realistic way, that is by moving the propellers and slightly bending tha chassis; moreover we attached 2 ```SceneCaptureComponent``` that respectively give the First-Person and Third-Person point of view of the drone. In the end we inserted two different sources of light in the drone: a ```SpotLight``` that points to the ground and represents the scanning view of the Drone, swapping color between green and red according to the perceived danger in the environment, and a ```PointLight```, which instead is placed on the top of the drone (within a glass sphere) that swaps between red and blue colors when a crime is detected, mimicking a police siren. To make it look like a real police patrolling drone we also added realistic sounds of a drone motors as well as of the siren.

Our Drone, which we used as a **Pawn**, has 3 main components that describe its behaviour: first it moves along the map through a pre-defined patrolling path, then during such motion it scans the environment through a camera to detect potential dangerous situations; in the end when a criminal is detected, the drone keeps track of such character maintaining it within its own field of view and alerting other citizens. The code that implements these different behaviours is all contained in the Blueprint of the Drone ([Drone_BP](https://unigeit.sharepoint.com/:u:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/Drone/Drone_BP.uasset?csf=1&web=1&e=8zeaxI)) and is described more in detail in the following sections.

### 1) Patrolling path
For moving the drone we used the ```FloatingPawnMovement``` Component as well as many ```Arrow``` components that we added to the drone: these allowed us to have some reference points for the drone thanks to which we draw a path along the narrow streets. Then we created 3 different Blueprint's functions that are ```Select Function``` ```Moving Function``` and ```Rotate Function``` and that are called on an **event based approach**: in particular at the beginning of the simulation, the **Select Next Location** Event gets called to start the behaviour of the drone, then the code calls the moving and rotate events that execute the related functions, to end with a loop call to the the **Select Next Location** Event; therefore this patrolling behaviour cycles forever until a crime is detected and a boolean flag (i.e. ```Detected Flag```) becomes true.

* **Select Function** : the first one is the function that gets called by the **Select Next Location** Event (as long as the ```Detected Flag``` is false), and is in charge of choosing the next goal of the drone. To do so it takes all the arrow components values and, after detaching them from the root component of the pawn (otherwise they would move together with the drone, therefore being unreachable), it puts them into an array; in this way these arrow are selected one by one and their _world location_ is extracted through a ```Get World Location``` block and assigned to a variable called ```Next Location```. After that we have in this part of the code the call to the **Moving** Event (which is described in the next section) and after which we simply have operation to update the index of the array's element to consider, taking into account that when the last position is reached the robot has to traverse the path in the opposite direction.
* **Moving Function** : this function is called by the **Moving** Event, and actually moves the Drone to the desired ```Next Location```  (i.e. the arrows locations as previously mentioned) by using the ```Set World Location and Rotation``` block. Morevoer to obtain the actual goal of the robot, the ```Next Location```gets linearly interpolated with the current location of the Drone (through the skeletal mesh component), using an alpha value that we tested on a trial & error approach and with which we obtained realistic and smooth movements. In the end this function calls the **Rotate** Event.
* **Rotate Function** : this function is called by the **Rotate** Event and simply sets the _World Rotation_ of the skeletal mesh using the ```Find look at Rotation``` function, that allows to find the required rotation such that drone is actually looking towards its moving direction



-----usato frecce per creare path predef da seguire....spiega codice :funzione usata (quale e perchè) e come abbiamo ottenuto movimento smooth tramite settare il delay e parametri come alpha

### 2) Environment Perception
per detectare pericoli abbiamo usato AI perception component settando sight come senso chiave...ogni volta che un nuovo cristiano viene detectato viene tentato un cast al Bluepirnt del delinquente..in questo modo quindi la detection è specifica di quel cristiano e non effettua un reale image recognitio/processing....spiega codice E SPIEGO ANCHE LE TELECAMERE QUA

### 3) Character Chasing

	
## Crowd


## Criminal Person

### The Map
====================================================================
The 3D environment representation of the 2D map of former assignment, which has been used for this project is shown here below:

<p><p align="center">
<img src="https://github.com/claudio-dg/assignment2/blob/main/media/GazeboMap.png?raw=true" width="500" />
<p>
