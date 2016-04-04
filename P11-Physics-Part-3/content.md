---
title: Physics Tuning
slug: physics-tuning
---

Now you'll learn how to setup the physics body *Contact Masks* and use that knowledge to remove seals when they are crashed into by penguins or ice blocks!

#The Physics contact delegate

SpriteKit already takes care of moving objects around when they collide. What you as developer need to do is to add *meaning* to different kinds of collisions. In our game for example, a meaningful collision is one between a *seal* and any other object in the game.

In SpriteKit we can do this by implementing a certain delegate. In Swift the concept of delegates is always used when one object signs up to get informed on certain events. The one we need to use to get informed about collisions in SpriteKit is called *SKPhysicsContactDelegate*. Let's add this protocol to *GameScene.swift*, so that we can get information on the collisions going on in our game.

> [action]
> Change the beginning of the GameScene class to implement the *SKPhysicsContactDelegate*:
>
```
class GameScene: SKScene, SKPhysicsContactDelegate {
```
>

Now that we've adopted the contact delegate protocol, we need to sign up to the contact delegate.

> [action]
> Add this line to `didMoveToView(...)`:
>
```
/* Set physics contact delegate */
physicsWorld.contactDelegate = self
```
>

#Physics masks

In SpriteKit each *physicsBody* has a property called *categoryBitMask*. This category is typically used to identify different participants in a collision.

You remember that we setup our penguin with a **Category Mask** of `1` and our seal with `2`. When a collision takes place we will be checking the *categoryBitMask* for a value of `2` to identify when a seal has been involved.

By default SpriteKit will not inform you of any collisions, I'm sure you've noticed the *Contact Mask* property when setting up your physics bodies.  By default they are all `0` so our *contactDelegate* will never be informed.

Our use case is straightforward, if anything hits a seal (even seal on seal) we want to know about it.

> [action]
> Open *Seal.sks* and set *Contact Mask* to `1`.

Now we will able to informed when any seals participate in a collision.

#Implementing a delegate method

Now we need to implement the *beginContact(...)* delegate method that will inform us when any collisions occur.

> [action]
> Now add this method to our *GameScene* class:
>
```
func didBeginContact(contact: SKPhysicsContact) {
    /* Physics contact delegate implementation */
>
    /* Get references to the bodies involved in the collision */
    let contactA:SKPhysicsBody = contact.bodyA
    let contactB:SKPhysicsBody = contact.bodyB
>
    /* Get references to the physics body parent SKSpriteNode */
    let nodeA = contactA.node as! SKSpriteNode
    let nodeB = contactB.node as! SKSpriteNode
>
    /* Check if either physics bodies was a seal */
    if contactA.categoryBitMask == 2 || contactB.categoryBitMask == 2 {
>
       print("Seal Hit")
    }
}
```
>

Run the game and cause a collision with a seal. You should see the log message appear in the console.
You may notice you a few seal hits at the start, currently it's super sensitive, even adding the *Level1.sks* to the scene the sealw will mostly likely, gently collide with the ground.

#Remove a seal when it get's hit hard

Now that we know that the collision handler is working, let's implement the actual functionality. Based on the *collisionImpulse* of the collision, we will decide if a seal will be removed or not. We're also going to set up a separate seal removal method, because we will want to add some more functionality to it.

> [action]
> Replace
>
```
print("Seal Hit")
```
>
> with
>
```
/* Was the collision more than a gentle nudge? */
if contact.collisionImpulse > 2.0 {
>
    /* Kill Seal(s) */
    if contactA.categoryBitMask == 2 { dieSeal(nodeA) }
    if contactB.categoryBitMask == 2 { dieSeal(nodeB) }
}
```
>

What exactly are we doing here? First we retrieve the kinetic energy of the collision between the seal and a second object. If this energy is large enough we decide to remove the seal by using the *dieSeal* method.

> [action]
> Finally add the *dieSeal* method to your code:
>
```
func dieSeal(node: SKNode) {
  /* Seal death*/
>
  /* Create our hero death action */
  let sealDeath = SKAction.runBlock({
      /* Remove seal node from scene */
      node.removeFromParent()
  })
>
  self.runAction(sealDeath)
>
}
```
>

This code should be fairly familiar, yet why can't we just call `removeFromParent()` directly, why do we need to wrap it in an action?
Well in the physics world we need to ensure that the seal is removed at the right time, in the SpriteKit frame render cycle there is a post-physics step called *didSimulatePhysics* after the physics simulation is complete for the current frame.  It is at this point that it's safe to remove a physics body, otherwise you can run into problems with triggering collisions on nodes that have just been removed, resulting in a crash.  If we wrap this in an action, SpriteKit will ensure it's executed at the right time in the cycle.

#Summary

You've implemented the physics **contact delegate**, used the **categoryBitMask** to validate which physics body has collided and used additional information from the collision to only remove our seals if they reach a certain collision thresh hold.

The game is getting to the final stages, let's add a little bit of polish next.
