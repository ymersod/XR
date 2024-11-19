### AR Project
##### Task 1: Finishing gameplay loop & setting up multiplayer functionality ingame 
##### Task 2: Healthbar implementation on schoolmons

### VR Project
##### About the project
The idea for our VR project is to create a game in the _Foddian_ genre centered around climbing a mountain using physics.
The game is called **Climbing Over it** - and is heavily inspired by [A difficult game about climbing](https://store.steampowered.com/app/2497920/A_Difficult_Game_About_Climbing/) and [Gorilla Tag](https://store.steampowered.com/app/1533390/Gorilla_Tag/).
With this in mind we expect that these will be some of the major systems/designs needed to be implemented in order to have a working MVP.

**_Objectives_**:
- Setting up VR project & connect our Quest 3(_We have one at home_) to develop the game
- Figure out how to use the tools / packages Unity provides for VR development
- Setting up hand tracking to move around without controllers
- Creating an appealing world to immerse the user
- Creating an interesting level(_mountain_) for the player to climb
- Figure out how to use physics in VR games

##### Task 0: Init the project & setting up link between headset and PC
###### Init project
When initting the project we wanted to have something to work with at the start, therefor we decided to start up using Unitys built in _VR Core_ template. With a template in place we tried to connect with our headset using _Meta quest link_, however some hardware / internet issues made it so we had to use _Meta Quest developer hub_ instead and test our project by directly building to the headset through a USB-C cable.

This makes it harder to debug or adjust values on the fly - but at least we can test the application at home :)

We decided to deviate from the initial plan of working from the _VR core_ template, as it had some innate rendering project settings and a metric ton of package bloat, that we didn't need nor knew how to get started using. Therefor we made an empty _3D URP_ project and worked our way slowly from there, as to get more control of settings / imports.

###### Terrain 
As one of our projects was to create an appealing world we wanted to create some sort of immersive terrain. Since the objective of the game is to climb a moutain we thought it made good sense the **3D Object Terrain**.
This object includes a **Terrain** component, which allows us to mold a natural field by hand and create something resembling a real mountain. We accomplised creating a mountain in our terrain by using the built-in _Paint terrain_ tool in the **Terrain** component.

After molding the terrain we wanted to make the environment more immersive by adding some sorrounding trees / objects & color. For this segment we imported
- _environment models_: [low-poly-simple-nature-pack](https://assetstore.unity.com/packages/3d/environments/landscapes/low-poly-simple-nature-pack-162153)
- _terrain textures_: [Polyhaven](https://polyhaven.com/textures/terrain)

The _environment models_ were used in the _Paint trees_ tool in the **Terrain** component, by inputting the models we want to use for our enviroment as _Trees_, we can make auto generate a huge immersive forest randomly using the models in _Trees_ section.

The _terrain textures_ were used to paint the terrain to make the colors resemble how a real mountain location would look. This was done by using the **Terrain** component, and using the _Paint Texture_ tool.

Below is the final result of the terrain from different angles

![image](https://github.com/user-attachments/assets/fcd73ad0-00f1-4462-8ca8-01dccf1373b1)
![image](https://github.com/user-attachments/assets/dcaee8e3-8f51-490c-b0fa-b5150ea490e2)
![image](https://github.com/user-attachments/assets/418f015d-2c99-4844-9e3c-3b15fa8235b9)

##### Task 1: Hand tracking & XR Rig
To immerse the user in the application we thought it would make sense for the user to climb with their hands and not with their controllers. Most modern VR Headsets has built in body tracking, so it should be possible to implement(_hopefully)_.

To accomplish Hand tracking we're going to be using these packages
- XR interaction tool kit
- XR hands

The **XR Tool kit** provides us with a prefab of a _XR Rig_, that we can use as inspiration when creating our own rig. However to visualise our hands in a fully virtual environment with this _XR Rig_, we need to also use **XR Hands**. This package contains tools to visualise the hands. 

There's not a lot of work for us to do here right now - as by using these packages in combination makes it very close to plug and play. However we give up a lot of control this way in how our rig operates, and later down the line we want to create our own _Climbing rig_. 

We decided to leave this part as is for now, and come back later after creating a physics system, since after playing around with the _XR Rig_ and some of its settings we found that applying a physics system to the existing _XR Rig_ seemed to be quite the undertaking, and very poorly documented as we're working with a new version of the **XR Tool kit**, that hasn't been explored thorughly online yet. Then later we can come back and slowly add components to our _Climbing Rig_. Below is a scrrentshot of the the devil that is the _XR Rig_ hierachy.

![image](https://github.com/user-attachments/assets/5658f573-b974-4eaf-82c5-c8c786e3c379)

##### Next steps
###### Physics system
We NEED to make a physics system on a rig asap, this is the core of the project.
- We don't really have a concrete plan for this yet as there's not a lot of material online, we have to improvise a bit here
- All we know is we need to figure out how to make a _RigidBody_ interact seamlessly with a rig somehow
- Also some kind of math to get momentum like in _A difficuly game about climbing_

###### Level creation
We want to create a finished level as to create a finished gameplay loop.
- Should be able to climb up to the top
- Should be able to fall down
- _Maybe be able to grab stuff along the way up / down_

###### Upgrading the world design
We want to make the world more immersive
- Adding cloud system to make the user feel like they are VERY high up in the air when climbing
- Adding sound system to make the interaction with the world feel more satisfying and real
