---
title: Particle Effects
slug: particle-effect
---

Now you are going to add an Angry Bird style particle effect whenever a seal gets eliminated. SpriteKit has a great integrated particle effect designer which we are going to use to define the style of our first particle effect.

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
> Add the following code to the start of the `didSeal` method:
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

#Summary

Well done! Now you know how to create particle effects and make them play when certain events in your game occur.

In the next chapter you will be adding animated penguins to the sidelines.
