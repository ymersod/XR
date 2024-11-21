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

### Task 4: Den anden siimooooon
:)
