#### Project as a whole
Very early on our process formed around us working on our individual parts of the project at home, and then meeting up every tuesday to talk about last weeks progress as well as discussing what each of us were going to be working on next.
To me this process ensured freedom of when to get the work done which is nice, however it came with the caviat of it sometimes not feeling like a group project. However we often did only have time to meet on tuesdays, and the lab was always busy - which to me at least makes it harder to get work done.
We met up at home to work on some stuff a couple of times which i really like, but i feel like a bit more involvement in each others problems and development would have been great for us.
I still feel like we made the right choice with our workflow though, as i don't really see the 'working together on the project at all times' approach working well for us.

#### My contributions
Most of my contributions lie in the VR part but for the AR project i worked on:

##### AR (10-20% of total contributions to the projects)
- 3D Model
I wanted to explore what the possibilities were for creating 3D Models using AI, and if it made more sense that creating a 3D Model from scratch. I provided the AI's i tried with a .PNG of a character i wanted converted to a 3D model that could be imported to Unity. My conclusion on this part is that the free solutions at least are not mature enough for developers to take advantage of this technology yet. It feels like it both stayed to true the 2D aspect of the character and weren't creative enough to make a proper model from my sketch.

- Setting up multiplayer
My part here lied in doing all the setup for 2 users to be able to play our game together. More or less was just connecting the app to the photon cloud provider, and doing some VERY simple UI and basic code setup for the multiplayer to work.

##### VR (90-80% of total contributions to the projects)
- VR Rig setup
We had a bit of niche use case where we couldn't really use the provided XR Rigs given my packages. So i created our own Rig for our specific Use Case, with the big problem that it had to be able to integrate with our physics movement system.

- Core physics system
From the start we wanted the player to be able to propel themselves in direction based on physics. This is not easy it would seem.

- Environment design
Primarily worked on the terrain part.

- Level design
In tandem with the others :)

#### Technical issues
Okay I will more or less only be talking about my issues developing for VR for this part, as it's my main contribution.
Before i continue, i want to say im very sorry for all the dead code in the project - to me it's a reminder of all the approaches i tried to solve some of the problems below that yielded no result. 
Im not lying when i say this has propably been the most painful dev experience i ever had. I think i'm the only one on the entire fu***** internet that has made a physics system with the newest versions of XR Interaction toolkit and Unity 6.0.
It's a shame i didnt have more time to work on the vr core functionality as i finally feel like i know whats happening behind the scenes... at least it barely works :)

##### Package Versions
As hinted at above i believe we had to use the newest packages for XR Development, as older versions had either non-supported components or totally deprecated components. This resulted in a LOT of trial and error when creating the rig and implementing the components provided by the packages, as documentation was sparse - and other devs using the packages were non-existant. I found work-arounds in the end based on older implementation of components and guesswork, but 5-10 hours at least were lost trying to figure out how the new components were working. Again this wouldn't have been a problem had we not decided to do some major changes to the core foundation of the XR-rig

##### Black-box logic
This ties into the next problem which is that a lot of the components used is from packages that from the inspector view isn't very self-explanatory. It's very hard to know what is needed and what is not when the base XR-rig provided has 50+ components spread out the entire prefab. In the end i did figure it out however, which is why i build our own XR-Rig, with only components what we need for our game. At least i know how the components interact and what their purpose is now :)))) 

##### Meta Quest Link
Some of this pain could have been circumvented had my air link i used on my Quest 3 at home worked... However i don't know if my internet is just that bad or what is wrong with my headset, but i couldn't for the life of me get quest-link to work. I resorted to building directly to my headset through meta quest devoloper hub and a usb-c cable, but without proper debugging tools this was another blocker i had to deal with when developing, and also using 1-2 minutes on each build sucks...

##### Physics x XR Rig locomotion
This is straight devillishly hard to accomplish. I at first tried to make my own physics system, but this was before i knew how the XR-rig worked and it was done i vain. I think what my biggest problem is that i didnt know how the built in locomotion system worked, and it was only after creating my own rig, and later adding a bit of locomotion i figured out why. Apparently rigidbody physics movement and the character controller movement is like oil and water. Having them both active at once provided disastrous bugs, but i ended on a solution disabling the physics movement and rb when using the locomotion system. Optimally i think we shouldn't use the locomotion at all, but this would require us to figure how the entire locomotion system works, and somehow transform that knowledge to physics movement so we can still climb on rocks in the game. We did not have time for this unfortunately.

##### Hand tracking ;(
Making the game work with hand tracking was a major objective for our project. I think i worked on this in like 3 iterations each time coming close to something working but not enough. I found The XR-rig components and the way they interact to be very rigid, and hard to modify or implement supplementing logic. 
- **First try** Using colliders directly on hands and adding a rb totally messes with the handtracking and the virtual movement of the players hands, also without a rigidbody the hands sometimes go through other colliders, even when using _continious dynamic_.
- **Second try** Using square hands on hand tracking as done with controllers. This actually worked, although we had to do a bit of different physics system for when using hands vs controllers. This didnt feel right though as we wanted the player to actually use their hands for real.
- **Third try** Constantly updating a mesh exactly representing the virtual hand with a mesh collider updated at runtime, we did this by baking a mesh based of the XR-rig hands skinned-renderer componenent. This is the closest i got to a solution, but as one would expect this costs way to much on performance, although it did work, this was not the way sadly. I think i would like to try representing the hand as a composite collider based on capsule colliders if i had time, this should improve performance, while still keeping structure of hands.
