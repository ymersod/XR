# Simon Sohn Christensen - Personal Reflections

So, now we have "_finished_" both the AR and VR project. I'll simply go over what I've felt/learned from each in their own category.

## AR - Schoolmon

So, in our group I got the responsibility of making sure this was in a running state doing the exhibitions.

Schoolmon uses img tracking to give position of the virtual world within real space. Now, here are a couple of issues I've encountered with the image tracking provided through Unity:

- Even though the rotation and the position tracking worked fine on our iPhones, the experience on Android phones in the lap was less consistent. They would from time to time do some unpredictable rotation on the cards, which would make some of the logic go weird, and the position was often lower than what was observed in real life
- The img tracker didn't update frequently enough, so moving things around could seem a bit laggy
- Had to get really close to the img to start tracking

Now, after some research, there are some things I would do differently.

1. I would look for other tracking options. From what I've seen, some people say there are better tracking options with faster respond time and further tracking capabilities.
2. Since we are using fairly small cards, look into how trackable the img is, is the contrast points hight enough for the tracker to know the rotation and so on.
3. Don't know if it would have helped much, but would have loved to see how feel the phones did phone tracking with the gps.

Other than that, when it comes to the code... its not the prettiest code ever written. Mostly thinking about Network management and the states monsters can be in. Here I would have love to generalize the code and separate the network calls into a more abstract way. As of right now its pretty hard to implement more things, since its all so entangled (_spaghetti code_).
Don't think its the worst code ever for a mvp, but if one would ever want to make a product out of this, the best solution would be to start over.

Things I would like to have implemented? I would have loved to implement shared tracking! Right now each phone does the tracking themselves. There is implemented shared tracking of health, but feel position and rotation of monsters would have been cool. This would also give a better feel that the players are in a shared world.

The plane manager was also an idea, to detect some planes and do some shaders to hide the monsters from view if you shouldn't be able to see them. But this was backlogged. Also found that on some of the android phones the room created would fly away if my finger went in front of the cam.

Other than that, I had some technical difficulties implementing some shaders onto the card. I wanted the monsters to pop out of the cards as if they came out of a pool. There are some shaders in there, from some experimenting with it. But in the end I went back to networking and monster movement as i felt it took up time.

## VR - Climbing over it 2

I don't really have anything to say here code wise...

As said in the final blogpost I primarily functioned as a game tester doing this part.

I did contribute ideas for fixing certain bugs and participated in pair-programming sessions(_as an observer_).

While we had planed for me to take the responsibility for the AR portion, and having less to do with the code in VR, I would have have liked to get more hands-on experience with coding for VR as well - getting a bit more "_dirt under my nails_" so to speak :)

But I did learn though playtesting how important frame rate is for delivering a comfortable experience. In the game, you can hit your hands together, transitioning from controller tracking to hand tracking. However the implementation of hand tracking slowed down the game substantially. One did feel a bit uncomfortable going so drastically from one pace to another.
