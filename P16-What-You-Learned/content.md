---
title: What You Learned
slug: what-you-learned
---

Congrats again on finishing *Peeved Penguins* and building another iOS game.

Let's take a look at what you have learned.

![The game](../cover.png)

##Physics, Physics, Physics

- **Static vs. Dynamic**: Static bodies don't move when collisions are calculated, so you should use them when creating something that will never change (i.e. the ground). Dynamic bodies are the opposite - they move and interact with their environment according to the laws of your physics node.

- **Polygon (Alpha) vs. Round**: At a low level, physical simulation is actually a set of equations being calculated every frame. Every calculation takes up processing power. More calculations are needed for polygon objects than round objects. More calculations are necessary still for a polygon object with many points than a polygon object with few points. Keep in mind that as you shape your physics objects to mach your sprites, it may be worth it to use a simpler shape rather than alpha if you can still achieve a enough precision in collision detection.

- **Using physics joints**: Physics joints are used to hold two physics bodies together with various constraints and modifiers.

- **Physics as visual polish**: The physics simulation itself is quite realistic, but you often need tricks to make the game appear more realistic. Keep in mind that even if the physics joints may not affect gameplay, they may still have a place in adding to a game's visual appeal.

- **SKPhysicsContact**: This object possesses information about the two colliding physics bodies. You can use its properties (collisionImpulse for example) to customize interactions.

- **Tweaking**: Tweaking physics values is essential to get the balance right in your game.

##Performance

- **Texture Atlas**: You learned the performance benefits of adding game assets into one large texture atlas to improve both loading time and game rendering.

##Game Content

- **Level Design**: A good level based game needs lots of level content, separating level design from your game engine code is essential. You don't want game mechanics to be part of the level design or it will become difficult to make any changes.  It also allows you to create new game content that could be downloaded from the cloud.

##Game Mechanics

- **touchesBegan/Moved/Ended**: You implemented touch controls that went beyond a simple touch, you tracked the players touches from start to finish, dynamically creating joints and using touch control to pull back the catapult.

- **Camera node tracker**: You learn to create custom code to allow the camera to track game objects while applying logic to ensure the camera did not go outside of the game scene.

##Game Polish
- **Particle effects as visual polish**: To give a game a professional feel, some visual response is necessary in almost every interaction. Particle effects are a great way to create the movement and liveliness needed to polish your game.

- **The gist**: Particle effects are versatile. Experiment a little; you can customize essentially every part to create a unique effect.

- **Sprit frame animations**: Setting up more complex sprite frame based animations and using action properties such as start time to make a group animation look more natural.

- **SFX**: Sound plays an important part in enhancing the game experience for the player, good use of sound can really help add that professional feel to a game.

##Device Compatibility
- **iPad compatibility**: SpriteKit makes it easy to add iPad compatibility with minimal effort.

#Solution

[Download Peeved Penguins](https://github.com/MakeSchool-Tutorials/Peeved-Penguins-SpriteKit-Swift-Solution).

![Github Cat](https://static.makegameswith.us/gamernews_images/TVZ2mTmQpl/labtocat.png)
