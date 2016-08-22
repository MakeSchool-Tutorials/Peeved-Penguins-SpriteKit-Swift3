---
title: Penguin launcher
slug: physics-penguin
---

Ready to try out the catapult on the penguins?

You already know how to dynamically add a new penguin to the scene, this time when you add a new penguin you're going to pin joint it to the catapult arm and then release it when the player lets go of the catapult arm and watch physics in action.  Let's add a new *penguinJoint* for this purpose.

#Pinning the penguin

> [action]
> Add this property to your *GameScene* class:
>
```
var penguinJoint: SKPhysicsJointPin?
```
>

When the player touch begins, you want to spawn a penguin and pin it to the bucket area of the catapult arm.

> [action]
> Add this code to the `touchesBegan(...)` method immediately after the **touchJoint** code:
>
```
/* Add a new penguin to the scene */
let resourcePath = NSBundle.mainBundle().pathForResource("Penguin", ofType: "sks")
let penguin = MSReferenceNode(URL: NSURL (fileURLWithPath: resourcePath!))
addChild(penguin)
>
/* Position penguin in the catapult bucket area */
penguin.avatar.position = catapultArm.position + CGPoint(x: 32, y: 50)
>
/* Improves physics collision handling of fast moving objects */
penguin.avatar.physicsBody?.usesPreciseCollisionDetection = true
>
/* Setup pin joint between penguin and catapult arm */
penguinJoint = SKPhysicsJointPin.jointWithBodyA(catapultArm.physicsBody!, bodyB: penguin.avatar.physicsBody!, anchor: penguin.avatar.position)
physicsWorld.addJoint(penguinJoint!)
>
/* Set camera to follow penguin */
cameraTarget = penguin.avatar
```
>

Most of this should be fairly easy to understand for you by now.  The code is very similar to the original beta launcher.  The manual impulse has been removed and replaced with create a pin joint between the penguin and the catapult arm.

## Let it go

Next you need to remove the *penguinJoint* once the player removes their touch.

> [challenge]
> Can you remove the *penguinJoint*?

> [solution]
> Add the following to the`touchesEnded(...)` method:
>
```
if let penguinJoint = penguinJoint {
  physicsWorld.remove(penguinJoint)
}
```
>

Run the game... You should be able to launch some penguins, good job!

# Fine Tuning

It's very important that everything feels right for the player.

Tuning your physics simulation is very important, what might be realistic in the real world. May not be much fun in a game.  You're aiming to get the balance just right and hit the sweet spot.

SpriteKit makes it very easy to change different physics properties, such as mass, friction, etc:

Have a play with your physics objects, sometimes the best results come from accidental changes.

# Summary

Well done! You have the first fully playable prototype build.

You've learnt how to:

- Launch a physics body indirectly using your catapult
- Tweaking physics to improve the simulation

In the next chapter you will be working with physics contacts and collisions.
