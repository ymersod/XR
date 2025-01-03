# Merethe Lundgreen – Personal reflection
It has been my first time trying to use both blender and unity, so that has been a learning path in itself. I didn’t know how to use the programs, and I didn’t know how to structure my workflow effectively at the beginning, but as the projects went on, I got more and more tools to work with.

## The AR part
So to kick it all off, I wanted to learn to make a 3D model, and that took a lot of hours and a lot of different youtube tutorials, until I had a basic understanding of vertices, edges, and faces, and how to manipulate these. It also resulted in a lot of weird shapes, that I hoped I could ignore, until I tried to import the mesh into unity. Here I could see, that it didn’t behave as I expected, because the geometry was weird. This led to learning more about mesh optimization, fixing normals, and cleaning up unnecessary vertices – and keeping in mind how to minimize the rendering cost in the future.

I also learned how to design from a reference image, and at the end of the AR project, I fell over retopology sculpting, which I really think that I should have used on Bugsy, but it is definitely a thing I will remember in the future!

Concerning the AR part and unity, I learned about anchor points, how to place and align 3D models on real-world surfaces. I also gave Bugsy bones and added a flying animation to him in Blender, then I had to import it into Unity. I used the Animator in Unity to set up the animation, creating an Animation Controller that allowed me to trigger the flying animation under certain conditions (or right now it is at all times, but I wanted to add an attack animation, but didn’t have the time unfortunately, so he just flies). 
At the beginning Bugsy kept flying forwards and upwards, so I used Unity's Rigidbody component to apply constraints and limit movement. I then made a script to ensure he flew more controlled, keeping him aligned with the AR environment and maintaining a consistent animation.

After the playing around in blender and adding my mesh and animation to unity, I set up the quiz logic, which gave me a bigger insight of how unity worked - especially how to connect things like buttons, animations, and scripts together, and how to use prefabs and techniques like event triggers and object pooling. Furthermore I learned how to structure the code, both concerning single responsibility, and the use of manager scripts. 

# The VR part
In the VR project, I started out by creating a cloud that spawns when reaching a specific height. I started out by making a simple flat disc in blender, with 100k vertices, so there was a lot to manipulate with. Then I imported it into unity, and I used shader graph to manipulate faces and vertices of the disc, to make it cloud like. I used a youtube tutorial to get started, to learn shader graph, and THAT was a whole new learning path. I think shader graph is quite technical, but when I got the hang of it, and began to understand the nodes and how to connect them, I felt a bigger understanding of it. I added movement and texture shifts to make the cloud feel dynamic. I also tried adding a fog like effect, using a combination of transparency and noise, but I didn’t really turn out as planned.

Although I feel like I’ve made an effort to get the hang of it, there were a few small issues that didn’t work as expected, and therefore still things to learn.

I also added a changing skybox, shadows, music (that changes during the day), hitting sounds. I got to play around with these, which felt real fun, and taking the user experience into consideration. 

## Wrapping it up
The movement in the VR world and networking in AR, I did together with the other two. We sat an entire day to get the movement and colliders correct in VR, and really diving into the specific functionalities of locomotion and rigs. This also gave me a big insight in how some of the techniques in VR works. The networking also took a lot of time and testing, and ended up overlapping the VR project, but ended up working as intended in the end! – and I’ve never been happier to have bought a macbook, as it allowed us to test it on our own phones, which had better cameras for AR detection.

All in all, I have learned a lot, and I really found it fun to do the projects! It hasn’t felt like a school project, but actually like a hobby project, where I used many of my failures as learning curves, and I have studied techniques to improve my designing, modeling and coding. I also ended up spending a lot of extra hours, so I could get a good understanding, of what I was doing. I have also learned a lot about XR, and design considerations, to make the projects be as nice for the user as possible. 

