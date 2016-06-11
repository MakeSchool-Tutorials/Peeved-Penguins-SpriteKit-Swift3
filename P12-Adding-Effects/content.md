---
title: Adding effects
slug: adding-effect
---

The game is looking good, it would be nice to start adding those little extra touches to enhance the game experience.  You are going to be adding both a particle effect and sound effect whenever a seal gets eliminated. SpriteKit has a great integrated particle effect designer which we are going to use to define the style of our first particle effect.

#Creating a new particle effect

> [action]
> Create a new particle effect `File > New > File > SpriteKit Particle File`
>
> ![SpriteKit Particle File](../Tutorial-Images/xcode_spritekit_add_particle.png)
>
> ![SpriteKit Particle File](../Tutorial-Images/xcode_spritekit_add_particle_template.png)
>

![SpriteKit Particle Black Smoke](../Tutorial-Images/animated_black_smoke.gif)

I ended up making a more subtle smoke effect. You can copy the property values shown or feel free to go crazy and create your own unique look.

> [action]
> Particle attributes:
>
> ![Particle Attributes 1](../Tutorial-Images/xcode_spritekit_particle_1.png)
>
> You can use the color ramp to create interesting cycles of color.
>
> ![Particle Attributes 2](../Tutorial-Images/xcode_spritekit_particle_2.png)
>

This should look something like that:

![SpriteKit Particle Grey Smoke](../Tutorial-Images/animated_grey_smoke.gif)

Feel free to spend some time playing around with different values.

##Adding the particle effect to our collision

Let's add some code that adds the particle effect to the scene whenever a seal gets eliminated.

> [action]
> Add the following code to the start of the `dieSeal` method:
>
```
/* Load our particle effect */
let particles = SKEmitterNode(fileNamed: "SealExplosion")!
>
/* Convert node location (currently inside Level 1, to scene space) */
particles.position = convertPoint(node.position, fromNode: node)
>
/* Restrict total particles to reduce runtime of particle */
particles.numParticlesToEmit = 25
>
/* Add particles to scene */
addChild(particles)
```
>

You load a particle effect and place it at the seals's position directly before the seal is removed from the scene. The runtime of the particle can be controlled by setting the *numParticlesToEmit*, it's not super logical so it takes a bit of trial and error to get a value that looks and feels right.

#Adding SFX

Adding sound effects is quite straight forward in SpriteKit, you can make use of the `playSoundFileNamed` *SKAction* to play sounds.

Download the [SFX Pack](https://github.com/MakeSchool-Tutorials/Peeved-Penguins-SpriteKit-Swift/raw/master/SFX.zip) we created for you. Once the download is complete, unpack the folder and add
to the project.

> [action]
> Add the following to the `dieSeal` method:
>
```
/* Play SFX */
let sealSFX = SKAction.playSoundFileNamed("sfx_seal", waitForCompletion: false)
self.runAction(sealSFX)
```
>

Run the game... Let there be sound!

#Summary

Well done! You learnt to:

- Create your own custom particle effects and make them play when certain events in your game occur
- Play SFX

In the next chapter you will be adding animated penguins to the sidelines.
