---
title: Working with contacts and collisions
slug: physics-contact-collisions
---

Next you'll learn how to setup the physics body *Contact Masks* and use that knowledge to remove seals when they are hit by penguins, ice blocks or even each other.

#The Physics contact delegate

What you as developer need to do is to add *meaning* to different kinds of collisions. In the game a meaningful collision is one between a *seal* and any other object in the game.

In SpriteKit you can do this by implementing the *SKPhysicsContactDelegate*. Let's add this protocol to the *GameScene* class, so you can get information on the collision going on the game.

> [action]
> Modify the GameScene class declaration implement the *SKPhysicsContactDelegate*:
>
```
class GameScene: SKScene, SKPhysicsContactDelegate {
```
>

Now that you've adopted the contact delegate protocol, you need to sign up to receive events generated
by the contact delegate.

> [action]
> Add this code to `didMoveToView(...)`:
>
```
/* Set physics contact delegate */
physicsWorld.contactDelegate = self
```
>

#Physics masks

In SpriteKit each *physicsBody* has a property called *categoryBitMask*. This category is typically used to identify different participants in a collision.

You remember that you setup the penguin with a **Category Mask** of `1` and the seal with `2`. When a collision takes place you will be checking the *categoryBitMask* for a value of `2` to identify when a seal has been involved in a collision.

By default SpriteKit will not inform you of any collisions, I'm sure you've spotted the *Contact Mask* property when setting up physics bodies.  By default they are all `0` so our *contactDelegate* will never be informed of any collisions taking place.

This use case is pretty straightforward, if **anything** collides with a seal (even seal on seal) you want to be informed.

> [action]
> Open *Seal.sks* and click on the seal, set *Contact Mask* to `1`.

Now anytime a seal is involved in a collision you will be informed through the contact delegate.

#Implementing a delegate method

You will need to implement the *beginContact(...)* delegate method that will inform you of any valid collision contacts.

> [action]
> Add this method to the *GameScene* class:
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

Run your game...

You should see the log message appear in the console every time a seal is hit.  You may notice you get a few seal hits at the start, currently this code is super sensitive, the act of adding *Level1.sks* to the scene will mostly generate a contact event as the seal gently touches the ground when they are first added to the scene.

#Hitting on seals, hard

Now that you know that the collision handler is working, let's implement the real functionality. What you want is a way to check how hard the seal was hit and use that to decided if you should ignore the collision or destroy the seal.  

SpriteKit to the rescue, you can utilize the *collisionImpulse* property of the collision to gauge the resulting impact force from the collision.

To keep things clean let's setup a separate seal removal method. You'll be adding more functionality to this method later.

> [action]
> Replace:
>
```
print("Seal Hit")
```
>
> with:
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

##Removing the seal

First you retrieve the collision impulse between the seal and the other object. If this impulse is large enough you decide to remove the seal by using the *dieSeal* method.

> [action]
> Add the *dieSeal* method to your *GameScene* class:
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

This code should be fairly familiar, yet one does not simply call `removeFromParent()` directly...

In the physics simulation you need to ensure that the seal is removed at the right time, in the SpriteKit frame render cycle there is a **post-physics** step called *didSimulatePhysics* after the physics simulation is complete for the current frame.  At this point it's safe to remove a physics body, otherwise you can run into problems with triggering collisions on bodies that have been removed, resulting in a horrible game crash.  If you wrap this in an *SKAction*, SpriteKit will ensure it's executed at the right time in the render cycle.

#Summary

The game mechanic is nearly finished, you've learnt to:

- Implement the physics *SKPhysicsContactDelegate*
- Use the **categoryBitMask** to identify physics bodies in a collision
- Use **SKPhysicsContact** to evaluate the collision force between two bodies
- Remove the seal cleanly from the scene

In the next chapter it's time to add a little polish.
