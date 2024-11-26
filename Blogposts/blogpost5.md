# AR project
### Task 1: SImooooon
(:

# VR project
### Task 2: Clouds (Merethe)
##### Creating the Cloud
To create the cloud, i made a 100k mesh in Blender. I started by making a cylinder with zero height. Then i subdivided the triangles and made the outer edges "to spheres" so i could later add curvature.

To ensure my mesh fit the scene, i set the length and width to 500m and scaled the disk so it was 1, 1, 1. The file was saved as an .fbx file and could now be used in Unity.

![image](https://github.com/user-attachments/assets/0d7f3aef-0149-491c-b9f1-5599dc88390f)![image](https://github.com/user-attachments/assets/67484868-b960-4f37-a851-0cce416d2724)

##### Texture and Projection
Once the disk was in Unity, i added a shader to give it a "cloud-like" texture and animation effect. I added the following:

* Gradient noise, which gives the texture to mesh. This has an associated noise scaler property to easily adjust how much noise is present in the scene.
* Tiling and offset, which ensured that the texture was repeated correctly across the disk.
* Position node, which gives access to the mesh’s vertex positions.
* Rotate around axis node, which ensures the noise pattern is not only one-dimensional, using a vector 4 axis node. This defines the axes which it can rotate.
![image](https://github.com/user-attachments/assets/6777a0d4-eead-4be4-ac4d-d3de09820c75)
(Initial image from the start setup (when it was a little more simple (: )

##### Animation
I also wanted to animate the materials, so i added a time node and a multiply node, which made it move. The multiply node controls the speed at which the clouds move, with a property that makes this editable in the scene view.
![image](https://github.com/user-attachments/assets/8368e433-b330-48bd-861b-ff709116ae77)

##### Vertex Offset
This gives position manipulation of each vertex
Using:
* Normal vector: provides the normals for these vertices
* Multiply normal vector: takes each vertex’s normal and multiplies it by the gradient degree, which determines which vertices are visible (giving a value between 0 and 1).
* Noise height property: determines the degree of displacement.

##### Noise Effect
To make the clouds more "blurry," i added another gradient noise but without movement.
![image](https://github.com/user-attachments/assets/c3af4bac-0a65-4fd6-8f11-51608e6a227f)

Giving this result:

![image](https://github.com/user-attachments/assets/91f1848a-4c51-4a45-964f-ae4c77410de0)
![image](https://github.com/user-attachments/assets/6e077b07-95ba-4bab-9a63-a2669514a3d3)


##### Avoid Uniform Shapes
To avoid too much uniformity in the clouds, i added a noise mod, where i remapped the vertex range and ensured that all negative values were made positive using the absolute node.

But i still wanted more non-uniform looking clouds, so i added another noise base, that i mixed with the other one.

![image](https://github.com/user-attachments/assets/ee139149-b742-4359-a901-831f89130a4c)

##### Coloring
To determine the colors for the highest and lowest points, i added a lerp with color valley and color peak properties.

##### Fresnel
Fresnel adds the "angle of incidence," which creates different reflections on the clouds depending on the viewing angle. This creates a nice lighting effect on the clouds. I added the properties "Fresnel power" and "Fresnel opacity", that determines the intencity of the reflection.

##### Curvature
To ensure the horizon doesn’t suddenly stop, i added a curvature that performs a set operation, adding curvature to the sphere effect of the mesh. This effect makes it look like the clouds continue in the horizpn.

##### Transparency
Finally, i added depth fade to create a transparency effect.
To make this work, i had to change my shader to be unlit instead of lit, so i could add this effect to the alpha.
![image](https://github.com/user-attachments/assets/4dc88831-5c9a-4946-b80b-fb68f6800826)

##### Final Thoughts
I tried to create a "fog" effect, but it didn’t work as i hoped. I think it needs to be set up correctly in unity, but after several attempts, i concluded it wasn’t worth the time as i was satisfied with the result.
But i was happy seeing the effects being applied:
![image](https://github.com/user-attachments/assets/94994a8d-000e-4290-b4ea-05b4c11b641d)
![image](https://github.com/user-attachments/assets/5b3597c9-c056-4f53-b4a4-4767b93cf0c0)
![image](https://github.com/user-attachments/assets/89453c48-2d42-4629-a826-e27d22529229)
![image](https://github.com/user-attachments/assets/8f25f57c-f597-42ff-aa6f-ac15a30bfc93)

In the last image, you can see that my mesh has been "mirrored" on half of its surface. I’m not sure how this happened, and i have tried to fix it in the shader, but i couldn't find the error, so it ended up like this..... (too many hours put into fixing it, not worth it. I think i may have something to do with the mesh, but not sure)

##### HighBasedSpawner.cs
To ensure the cloud doesn’t just float in the air all the time, the idea was for it to spawn when you’ve climbed a certain height. I created this script, which tracks the player’s height and ensures the cloud spawns and follows the player as they climb (so you can't see how far down it really is).
It has the following properties:

```csharp
    public float spawnHeight = 10f;
    public float heightTolerance = 0.5f;
    private GameObject spawnedMesh;
    public float distanceFromPlayerToMesh = 5f;
```
These are set based on when the cloud should spawn. heightTolerance is a threshold value that defines how close the player’s height must be to the spawn height for the mesh to spawn or despawn.
spawnedMesh is obviously the cloud, and distanceFromPlayerToMesh defines how far below the player the cloud should spawn.

Since i don’t have a VR headset at home, i used the XR device simulator to check if everything works as expected in game mode. (and it did, wooo :D - and it worked when testing in on Simon's)


### Task 4: Skybox (Merethe)
To make the game more visually interesting and provide a sensory experience, i created a skybox that changes as time passes.
![Untitled video - Made with Clipchamp (1)](https://github.com/user-attachments/assets/0c76a00b-ad79-4a37-b4b1-354cae0e95a6)


To set this up, if initially installed "Fantasy Skybox FREE" from https://assetstore.unity.com/packages/2d/textures-materials/sky/fantasy-skybox-free-18353.
Then under the shaders folder, i copied the shader code from here: https://pastebin.com/1CSJmbYH, and then made a material from this. 

Then i added the Skybox material so the scenery's skybox material under lighting.

![image](https://github.com/user-attachments/assets/f204315f-0a97-48ae-ba83-825bcf708667)


The skybox material has two textures, that it shifts between:

![image](https://github.com/user-attachments/assets/818530c5-e2b1-494c-983e-a978de74ed41)

Now to make a flow with the skybox and textures, i made a time manager script:

![image](https://github.com/user-attachments/assets/f1035730-58f4-4aef-b3cc-a87ad8a811c3)

The script uses four textures (skyboxNight, skyboxSunrise, skyboxDay, skyboxSunset) to represent different times of the day.
It also uses four gradients (gradientNightToSunrise, gradientSunriseToDay, gradientDayToSunset, gradientSunsetToNight) to control smooth color transitions for the global light and fog.

To manage the time, the script tracks time in minutes, hours, and days. It uses properties (Minutes, Hours, and Days) to detect changes and trigger events like skybox or light transitions.

The Update() method rotates the globalLight object to simulate the movement of the sun/moonn (or moon) across the sky.
It also makes time progress with tempSecond; Time.deltaTime is the interval in seconds from last frame to current one. By using this, i can determine how often i want the minutes to increase in this interval. The lower speedOfTime is, the faster time goes in the scenery. 

```csharp
    public void Update()
    {
        tempSecond += Time.deltaTime;

        globalLight.transform.Rotate(Vector3.up, (1f / (1440f / 4f)) * 360f, Space.World);

        if (tempSecond >= speedOfTime)
        {
            Minutes += 1;
            tempSecond = 0;
        }
    }
```
OnHoursChange, at specific hours (6, 10, 18, 22), it triggers:
* A skybox transition between two textures.
* A light color transition using gradients.
10f is the duration of the fading.
```csharp
    private void OnHoursChange(int value)
    {
        if (value == 6)
        {
            StartCoroutine(LerpSkybox(skyboxNight, skyboxSunrise, 10f));
            StartCoroutine(LerpLight(graddientNightToSunrise, 10f));
        }
        else if (value == 10)
        {
            StartCoroutine(LerpSkybox(skyboxSunrise, skyboxDay, 10f));
            StartCoroutine(LerpLight(graddientSunriseToDay, 10f));
        }
        else if (value == 18)
        {
            StartCoroutine(LerpSkybox(skyboxDay, skyboxSunset, 10f));
            StartCoroutine(LerpLight(graddientDayToSunset, 10f));
        }
        else if (value == 22)
        {
            StartCoroutine(LerpSkybox(skyboxSunset, skyboxNight, 10f));
            StartCoroutine(LerpLight(graddientSunsetToNight, 10f));
        }
    }
```

LerpSkyBox interpolates between textures, and LerpLight interpolates the light color. 

### Task 5: Sounds (Merethe)
I added an AudioScript, that changes background music, and makes a fade effect between the changes. 

```charp

    public AudioSource backgroundAudio;
    public AudioClip nightAudio;
    public AudioClip sunriseAudio;
    public AudioClip dayAudio;
    public AudioClip sunsetAudio;

    // Fade out the current audio, switch the clip, and fade it back in
    public void ChangeBackgroundMusic(AudioClip newClip, float fadeDuration = 2f)
    {
        if (backgroundAudio.clip == newClip)
            return; // Skip if the new clip is already playing

        StartCoroutine(FadeOutAndChangeClip(newClip, fadeDuration));
    }
```

So for each time of the day, a specific audio clip can be assigned, to fit that time. 
The TimeManager then get an instance of AudioScript, so the ChangeBackgroundMusic can be applied in the OnHoursChange method

```csharp

    private void OnHoursChange(int value)
    {
        if (value == 6)
        {
            StartCoroutine(LerpSkybox(skyboxNight, skyboxSunrise, 10f));
            StartCoroutine(LerpLight(graddientNightToSunrise, 10f));
            audioManager.ChangeBackgroundMusic(audioManager.sunriseAudio);
        }
        else if (value == 10)
        {
            StartCoroutine(LerpSkybox(skyboxSunrise, skyboxDay, 10f));
            StartCoroutine(LerpLight(graddientSunriseToDay, 10f));
            audioManager.ChangeBackgroundMusic(audioManager.dayAudio);
        }
        else if (value == 18)
        {
            StartCoroutine(LerpSkybox(skyboxDay, skyboxSunset, 10f));
            StartCoroutine(LerpLight(graddientDayToSunset, 10f));
            audioManager.ChangeBackgroundMusic(audioManager.sunsetAudio);
        }
        else if (value == 22)
        {
            StartCoroutine(LerpSkybox(skyboxSunset, skyboxNight, 10f));
            StartCoroutine(LerpLight(graddientSunsetToNight, 10f));
            audioManager.ChangeBackgroundMusic(audioManager.nightAudio);
        }
    }
```

Then i found some appropiate sounds, that i wanted to use, and i compressed and shortened them, so the file size wouldn't be too big.
Then our ground plane gets the Audio Script attached, including four different background sounds, and the music will play.
Time manager has a reference to the plane, and can then change the music accordingly.


I also added a sound effect for when hitting the terrain. This was simply added by attaching the HitSounds script, which takes an audioSource, and triggers it to play. I added the Audio Source component to the terrain itself, and also gave the terrain the hitSound script. 

Each hand then get a sphere collider component, where radius is set very low, so it doesn't repeat the sounds too much, and "is trigger" is enabled.
![image](https://github.com/user-attachments/assets/f34c1378-8193-4c88-8a45-1c604a7771e5)



Now whenever we hit the terrain, a hit sound will appear.

### Functionality
### The rig
Task 6,7 and 8 depends heavily on us getting the rig to work in the way we want to. The rig was slowly upgraded from a very basic xr-origin rig with only camera, to a fully working physics rig later. So to not jump to much back in forth we will look at the rig now:

![image](https://github.com/user-attachments/assets/a1582d7f-ce88-4d70-91f6-b24e3699c46f)

Going from top to bottom, it shows that we have a very sparse locomotion system. We wanted to constrain the users movements to only be using our own physics system, and thus all we needed was the functionality from the _climbing provider_ component provided by XR Toolkit. The problem here is that using the provided locomotion system requires a _Character Controller_(or another manually create component IXR Body Posistion Evaluator) which we would add another layer of functionality, we went for the character controller, as we know it works with the _climb provider_.

The main camera is default setup.

All left/right hand/controllers implement more or less the same logic for interactions. We only needed to be able to interact with objects and throw them around for our game so we set up a _Near-far Interactor_ on each gameObject and set the settings up ensuring only near casting. So all other components provided from XR-toolkit can be excluded for now.
![image](https://github.com/user-attachments/assets/5d173363-0474-4290-a493-af35811eb5f5)

The left/right hands also both contain an _interaction Visual_ gameObject, this gameObjects purpose are to render the hands of the player when using hand-tracking on a headset that provides that functionality. And has a major impact on Task 7. Also the hand tracking is dependent on the VR-hands package, which allows us to use the Hand Visualizer component to visualize the users hands at runtime.

At the end we have our Physics XR Rig gameObject. This is standalone manager esque object that handles switching between using controller physics hands and hand tracking physics hands, as well as climbing mode hands. We will go more in depth on this in task 7 and 8.

### Task 6: Physics system
The entire reason we decided upon creating our own VR-rig was because we found weird kinks/bugs and unknown stuff about the freely provided VR-rigs from Unity's packages.

So starting from scratch we first of all added a RigidBody to simulate gravity on the entirety of the rig. Hereafter we added a collider on the main camera so the user doesnt fall through the floor. But this is just the basic stuff.

The interesting stuff is how we're handling movement without a locomotion system on our rig. For the moment the system only works using the controllers, however the logic is implemented for hand-tracking but we didn't get it to a high enough quality in time unfortunately, so we'll be focusing on the physics using the controllers.

First The  Physics ControllerMode... objects in the **Physics XR Rig** follow the XR-controllers used in real life. It looks something like this
![image](https://github.com/user-attachments/assets/7fafcdf7-6c99-442a-87e4-0e7478b0e887)

They track the controllers using this movement script. This is mostly some boring physics stuff, the important part is that using script combined with a rigidBody and colliders on the Physics ControllerMode... ojects allow us to move the gameobject around representing exactly where the controller would be, while still respecting collision with the environment. 
```csharp
void PIDMovement()
    {
        float kp = (6f * frequency) * (6f * frequency) * 0.25f;
        float kd = 4.5f * frequency * damping;
        float g = 1 / (1 + kd * Time.fixedDeltaTime + kp * Time.fixedDeltaTime * Time.fixedDeltaTime);
        float ksg = kp * g;
        float kdg = (kd + kp * Time.fixedDeltaTime) * g;
        Vector3 force = (target_transform.position - transform.position) * ksg + (playerRigidbody.linearVelocity - _rigidbody.linearVelocity) * kdg;
        _rigidbody.AddForce(force, ForceMode.Acceleration);
    }
```

To actually move the player when the player slams the the ground with the Physics ControllerMode... objects we use a mathematical equation called HookesLaw, the jist is that that gameObject acts like a spring, and when when touching something that spring pops and propels the XR-Rigs rigidbody in the opposite upwards direction the hands were coming from.
```csharp
 void HookesLaw()
    {
        Vector3 displacementFromResting = transform.position - target_transform.position;

        Vector3 force = displacementFromResting * climbForce;
        float drag = GetDrag();
        
        playerRigidbody.AddForce(force, ForceMode.Acceleration);
        playerRigidbody.AddForce(drag * -playerRigidbody.linearVelocity * climbDrag, ForceMode.Acceleration);
    }

```

It's also important that this part of the code only works when grounded so we added a check on 


```csharp


```






### Task 7: Hand tracking physiscs
### Task 8: Climbing system
### Task 9: Level design

