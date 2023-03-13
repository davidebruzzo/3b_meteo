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
* [kids_park](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/kids_park?csf=1&web=1&e=KVdXnr): This folder contains the park games (girandola, slide etc.) which are placed into the playground.
* [main_Building](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/main_Building?csf=1&web=1&e=R9nZCM): Here there is the asset of the main building.
* [other_building](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/main_Building?csf=1&web=1&e=R9nZCM): All the other buildings like shops, vending machine place etc. are contained here.
* [street_lights](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/street_lights?csf=1&web=1&e=gNOOGe): Models of the lights that have been placed on the buildings within the alleys and from the lampposts around the square and in the street towards the church.
* [trashBin](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/trashBin?csf=1&web=1&e=JtJ3lG): Models of trash bin we edited with our colors in order to simulate waste sorting.
* [trees](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/trees?csf=1&web=1&e=CbgtFI): tree models we put in the playground.
* [vending_machines](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/vending_machines?csf=1&web=1&e=abxkfy): 3D representation of vending machine we put in a little store in the middle of the narrow streets, this building represents a H24 vending store.

Then we have the 3D model of the drone that we will explain better in the next section.

## Drone
First of all we took the model of a drone from internet and put all the related files in the "[Drone](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/Drone?csf=1&web=1&e=MM5g04)" folder.

For such drone we created our own [animations](https://unigeit.sharepoint.com/:f:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/Drone/Animations?csf=1&web=1&e=zpTkty) to make it move in a more realistic way, that is by moving the propellers and slightly bending the chassis; moreover we attached 2 ```SceneCaptureComponent``` that respectively give the First-Person and Third-Person point of view of the drone. In the end we inserted two different sources of light in the drone: a ```SpotLight``` that points to the ground and represents the scanning area of the Drone, swapping color between green and red according to the perceived danger in the environment, and a ```PointLight```, which instead is placed on the top of the drone (within a glass sphere) that swaps between red and blue colors when a crime is detected, mimicking a police siren. To make it look like a real police patrolling drone we also added realistic sounds of a drone motors as well as of the siren.

QUI FOTO DRONE

Our Drone, which we used as a **Pawn**, has 3 main components that describe its behaviour: first it moves along the map through a pre-defined patrolling path, then during such motion it scans the environment through a camera to detect potential dangerous situations; in the end when a criminal is detected, the drone keeps track of such character maintaining it within its own field of view and alerting other citizens. The code that implements these different behaviours is all contained in the Blueprint of the Drone ([Drone_BP](https://unigeit.sharepoint.com/:u:/r/sites/VRxRobot2223-Group3BMeteo/Documenti%20condivisi/Group%203BMeteo/MyProject/Content/3b_meteo/Drone/Drone_BP.uasset?csf=1&web=1&e=8zeaxI)) and is described more in detail in the following sections.

### 1) Patrolling path
For moving the drone we used the ```FloatingPawnMovement``` Component as well as many ```Arrow``` components that we added to the drone: these allowed us to have some reference points for the drone thanks to which we draw a path along the narrow streets. Then we created 3 different Blueprint's functions that are ```Select Function``` ```Moving Function``` and ```Rotate Function``` and that are called on an **event based approach**: in particular at the beginning of the simulation, the **Select Next Location** Event gets called to start the behaviour of the drone, then the code calls the moving and rotate events that execute the related functions, to end with a loop call to the the **Select Next Location** Event; therefore this patrolling behaviour cycles forever until a crime is detected and a boolean flag (i.e. ```Detected Flag```) becomes true.

* **Select Function** : the first one is the function that gets called by the **Select Next Location** Event (as long as the ```Detected Flag``` is false), and is in charge of choosing the next goal of the drone. To do so it takes all the arrow components values and, after detaching them from the root component of the pawn (otherwise they would move together with the drone, therefore being unreachable), it puts them into an array; in this way these arrows are selected one by one and their _world location_ is extracted through a ```Get World Location``` block and assigned to a variable called ```Next Location```. After that we have in this part of the code the call to the **Moving** Event (which is described in the next section) and after which we simply have operations to update the index of the array's element to consider, taking into account that when the last position is reached the robot has to traverse the path in the opposite direction.
* **Moving Function** : this function is called by the **Moving** Event, and actually moves the Drone to the desired ```Next Location```  (i.e. the arrows locations as previously mentioned) by using the ```Set World Location and Rotation``` block. Moreover to obtain the actual goal of the robot, the ```Next Location```gets linearly interpolated with the current location of the Drone (i.e. its skeletal mesh component), using an alpha value that we tested on a trial & error approach and with which we obtained realistic and smooth movements. In the end this function calls the **Rotate** Event.
* **Rotate Function** : this function is called by the **Rotate** Event and simply sets the _World Rotation_ of the skeletal mesh using the ```Find look at Rotation``` function, that allows to find the required rotation such that drone is actually looking towards its moving direction.


### 2) Environment Perception
To detect dangers in the environment we used the **AIPerception** Component, setting "sight" as main sense. Please note that in our simulation we defined a priori that the "danger" is represented by a criminal guy that holds a gun and randomly moves in the streets, so we know from the beginning that the criminal guy will belong to a specific AI class, that is the ```My_AI_PATH``` class. Therefore to scan we used the **On target Perception Updated** Event, which gets called each time the AIperception detects a new actor; in particular, each time a person is seen by the drone, it tries to cast such element to an element of ```My_AI_PATH``` class: if it fails to do so, it means that it detected a random "safe" guy, if instead the cast succeeds the Criminal has actually been recognised, so the ```Detected Flag```  is set to true and the target is put into a variable (```Bad Guy```), in this way the drone stops its **patrolling** routine and enters the **chasing** phase.


### 3) Character Chasing
This phase starts when the ```Detected Flag```  is set to true, in particulat the **Event Tick** constantly checks for the value of such variable, and as soon as it becomes true it calls **AAA** Event to chase the bad guy in an infinite loop (until the end of the simulation), also switching the siren colors. In this phase the drone keeps track of the criminal keeping a constant distance from him, and mantaining him in its field of view. To do so it makes us of an analogous algorithm to the one described previously in the ```Moving function```, but this time instead of giving the arrows location as targets, we use the criminal's location, retrieved from the ```Bad Guy``` variable, but setting a fixed z-coordinate to keep the drone above the carachter, and subtracting a low value to the x-coordinate so that it is slightly preceeds the criminal, mimicking a real chase. In the same way the rotation of the dron is handled in such a way that it always points towards its moving direction, while the First-Person POV camera is rotated so that it fits the target at the center of the image

### 4) About Drone cameras
For the cameras as said we used 2 ```SceneCaptureComponent``` that respectively give the First-Person and Third-Person point of view of the drone. For Their implementation we simply attached them to the skeletal mash of the drone and then created specific widgets with their images, also adding features such as a cross in the middle of the first-person camera. In the Blueprint of the Drone we then created at **Event BeginPlay** a first widget  for the 1st person camera adding it to the viewport, so that when the simulation begins, the drone's POV is shown in real time on the bottom right of the screen, while the player can still move freely in he environment. When instead a criminal is detected, we remove the previous widget and replace it with a new one which still has the Drone's POV on the right part of the screen, but which has the third person view fixed on the rest of the screen.


## Crowd

For creating the crowd walking into the streets, we started from different blueprints to create different types of people. 

They are built on the ```metahuman``` package and they have different height, skin, face, gender and also accessory. 
We thought about static characters also, which are for example people talking on the phone we provided with a specific animation. For each different character walking in the streets, we developed different **data asset** files and a **state tree** file.

These files represent the behaviour of the crowd, and we set them in order to spawn randomly in a given area and walking onto a pre-defined navigation path.

To accomplish this goal we draw the navigation lines using ```zone shape``` components which are linked together to create the ```Zone Graph``` which is the actual possible route people can walk in. 

The full path in which they can move is around the buildings, in most of the narrow streets, and around the pub. 
They can also move around the square and the fountain.

These character have also a personalized style of walking to make them more realistic.

Some of the static characters are distributed on the map, and their position is fixed from the beginnig since we costrained it from code. 
They can be found near the fountain, in some angles of the narrow streets and also into the H24 automatic vending store.


## Criminal Person

### The Map
====================================================================
The 3D environment representation of the 2D map of former assignment, which has been used for this project is shown here below:

<p><p align="center">
<img src="https://github.com/claudio-dg/assignment2/blob/main/media/GazeboMap.png?raw=true" width="500" />
<p>
