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

When the player touch begins, you want to spawn a penguin and then pin it to the bucket area of the catapult arm.

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

Most of this should be fairly easy to understand for you by now.  The code is very similar to the original beta launcher.  The manual impulse has been removed and replaced with create a pin joint between the penguin and the catapult arm.  This joint will be maintained until the player releases their touch.

##Let it go

As in the previous chapter, you need to release the *penguinJoint* when the player releases their touch.

Can you add this joint removal yourself?

> [solution]
> Add the following to the `touchesEnded(...)` method:
>
```
if let penguinJoint = penguinJoint { physicsWorld.removeJoint(penguinJoint) }
```
>

#Tuning

Now run the game and launch a couple of penguins! It's very important that everything feels right for the player.
Tuning your physics simulation is very important, what might be realistic may not be much fun.  You're aiming to get that balance just right and hit the sweet spot.

SpriteKit makes it very easy to change different physics properties, such as mass, friction, etc:

Let's try a tweak on our catapult, if you notice when the game starts the catapult looks like it's swinging in the breeze.  You can apply an easy tweak to make this look better.  

Let's remove gravity on the catapultArm, this will stop the see-saw effect.

> [action]
> Change this body property to:
```
catapultArmBody.affectedByGravity = false
```
>

Run your game... Looking tight.

#Summary

Well done! You have the first fully playable prototype build.

You've learnt how to:

- Launch a physics body indirectly using your catapult
- Tweaking physics to improve the simulation

In the next chapter you will be working with physics contacts and collisions.
