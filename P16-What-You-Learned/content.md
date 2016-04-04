---
title: What You Learned
slug: what-you-learned
---

#Physics, Physics, Physics

* **Static vs. Dynamic:** Static bodies don't move when collisions are calculated, so you should use them when creating something that will never change (i.e. the ground). Dynamic bodies are the opposite - they move and interact with their environment according to the laws of your physics node.
* **Polygon vs. Round:** At a low level, physical simulation is actually a set of equations being calculated every frame. Every calculation takes up processing power. More calculations are needed for polygon objects than round objects. More calculations are necessary still for a polygon object with many points than a polygon object with few points. Keep in mind that as you shape your physics objects to mach your sprites, it may be worth it to use a simpler shape rather than alpha if you can still achieve a enough precision in collision detection.
* **Using physics joints:** Physics joints are used to hold two physics bodies together with various constraints and modifiers.
* **Physics as visual polish:** The physics simulation itself is quite realistic, but you often need tricks to make the game appear more realistic. Keep in mind that even if the physics joints may not affect gameplay, they may still have a place in adding to a game's visual appeal.
* **SKPhysicsContact:** This object possesses information about the two colliding physics bodies. You can use its properties (collisionImpulse for example) to customize interactions.

#Particle Effects
* **Particle effects as visual polish:** To give a game a professional feel, some visual response is necessary in almost every interaction. Particle effects are a great way to create the movement and liveliness needed to polish your game.
* **The gist:** Particle effects are versatile. Experiment a little; you can customize essentially every part to create a unique effect.

#iPad Compatibility
* SpriteKit makes it easy to add iPad compatibility with minimal effort.

#Solution

The solution to this [Download Peeved Penguins](https://github.com/MakeSchool-Tutorials/Peeved-Penguins-SpriteKit-Swift-Solution).

![](https://static.makegameswith.us/gamernews_images/TVZ2mTmQpl/labtocat.png)
