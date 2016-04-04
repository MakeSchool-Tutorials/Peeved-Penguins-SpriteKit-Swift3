---
title: Penguin launcher
slug: physics-penguin
---

Ready to try out the catapult on our penguins?

We know how to add a new penguin to the scene, this time when we add a new penguin we're going to pin joint it to the catapult and then release it when the player lets go of the catapult and let nature take its course.  Let's add a new *penguinJoint* for this very purpose.

> [action]
> Add this property your the *GameScene* class:
>
```
var penguinJoint: SKPhysicsJointPin?
```
>

#Spawn a sticky penguin

When a touch begins, we need to spawn a penguin and pin it to the bucket of the catapult arm.

> [action]
> Add this code to the `touchesBegan(...)` method immediately after the touchJoint code:
>
```
/* Add a new penguin to the scene */
let penguin = MSReferenceNode(fileNamed: "Penguin") as MSReferenceNode!
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

Most of this should be fairly easy to understand by now, the code is very similar to the original test launcher.
We have stripped out the manual impulse and instead pinned the penguin to the catapult arm, waiting for the player to let go.

#Let it go

As in the previous chapter, we need to release this *penguinJoint* when the player releases their touch.
See if you can add this joint removal yourself.

> [solution]
> Add the following to the `touchesEnded(...)` method:
>
```
if let penguinJoint = penguinJoint { physicsWorld.removeJoint(penguinJoint) }
```
>

#Tuning

Now run the game and shoot a couple of penguins! It's important that everything feels right. If you like you can do a little of tuning now (an important part of building physics games). SpriteKit makes it very easy to change different physics properties, such as mass, friction, etc:

Let's try a tweak on our catapult, if you notice when the game starts the catapult looks like it's swinging in the breeze.  We can make a really easy tweak to make this look better.  Let's disable gravity on our catapultArm, this will stop it falling over and then being pulled by the spring joint, causing this see saw type action.

> [action]
> Disable gravity on the catpault arm.
>
```
catapultArmBody.affectedByGravity = false
```
>

Run the game... looking better :)

#Summary

Well done! Now you have a first fully playable prototype. You've learnt how to launch a physics body indirectly using your catapult mode.

In the next chapter we will start turning this into an actual game.
