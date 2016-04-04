---
title: Catapult physics
slug: physics-catapult
---

Now things are going to get real. We are going to implement a shooting mechanism. First I'll get started with a short introduction to some of the basics you'll need for this step.

##What are joints and why do I need them?

In this step we are going to turn the catapult and the catapult arm into physic objects. Unlike the other physics objects we have in our game at the moment, the catapult consists of two parts that we will need to keep together. Two physics objects can be connected to each other using joints.

Don't understand why you need joints yet? I'll give you a showcase.

> [action]
> Open *GameScene.sks* and turn catapult and catapult arm into physics objects. Use `Alpha mask` for the *Body Type*.
>

Now publish and run. After a couple of seconds your scene will look similar to this:

![Broken catapult](../Tutorial-Images/screenshot_broken_catapult.png)

The catapult falls apart because we aren't using joints to keep it connected! The good news is we can create joints directly in the editor, making the next step really easy.

#Getting ready for your first joint ;)

First, let's revise the physics body for the catapult.

> [action]
> Make the catapult (not the arm!) a **static body** in the right panel. We don't want the catapult to move around through the scene.
> Untick - **Dynamic**, **Allows Rotation** and **Affected By Gravity**.

Next we will remove the physics body from the catapult Arm as it would be a nice to learn how to implement a physics body in code.

> [action]
> Change the *Body Type* of the catapult Arm back to `None` to remove.
>

There is no support to visually create joints in our *GameScene* so we will have to code all of this, we can use the Scene Editor to create all of the physics bodies we wish to connect to rather than doing all of this in code.  Before we do this let's give ourselves
access to the catapult and catapult Arm nodes.

We already have a code connection for the **catapultArm**, can you connect the **catapult** as well. Remember to set the *Name*, I recommend **catapult** :]

Next we are going to manually create the *Physics Body* for the **catapultArm**.

> [action]
> Add the following to `didMoveToView(...)` after the level loading code.
>
```
/* Create catapult arm physics body of type alpha */
let catapultArmBody = SKPhysicsBody (texture: catapultArm!.texture!, size: catapultArm.size)
>
/* Set mass, needs to be heavy enough to hit the penguin with solid force */
catapultArmBody.mass = 0.5
>
/* No need for gravity otherwise the arm will fall over */
catapultArmBody.affectedByGravity = true
>
/* Improves physics collision handling of fast moving objects */
catapultArmBody.usesPreciseCollisionDetection = true
>
/* Assign the physics body to the catapult arm */
catapultArm.physicsBody = catapultArmBody
```
>

<!-- -->

> [info]
> *Remember* Apple documentation is your friend, if you are unsure of anything, highlight the method or property in question and look at the * Quick Help* description.
>

Run the game, you will notice the catapultArm simply falls over and knocks down some ice blocks.

#Pinning it together

Now time to create your first joint, we will be using a *SKPhysicsJointPin* to connect the catapult and arm together.

> [action]
> Add the following to the end of your `didMoveToView(...)` method.
```
/* Pin joint catapult and catapult arm */
let catapultPinJoint = SKPhysicsJointPin.jointWithBodyA(catapult.physicsBody!, bodyB: catapultArm.physicsBody!, anchor: CGPoint(x:220 ,y:105))
physicsWorld.addJoint(catapultPinJoint)
```
>

The *anchor* is relative to scene, so you want to find the **position** in scene space of the pivot point on the catapult. An easy way to do this is add a sprite to the scene and move it to the position you want to anchor on, use that position and then remove the node from the scene.

Now you have set up your first joint! Run the game...  

Your catapultArm should now gracefully fall over while pinned to the catapult, although launching penguins is a little odd :]

#Spring into action

We need a joint that will simulate the action of a catapult, allowing us to pull back on the bucket and launch forward.
An *SKPhysicsJointSpring* sounds like it would work nicely here, as the spring length is extended the more force will be required to bring the bodies back together.  However, we always need two physics bodies to create a joint.

It's common to create invisible physics bodies to serve this purpose, we can use Scene Editor to create an invisible *SKSpriteNode* and set it up with a static physics body.  This will then be used in the creation of the *SKPhysicsJointSpring*.

> [action]
> Drag a *Color Sprite* onto the stage and place it above the catapult arm.
> Set the *Name* to `cantileverNode` fancy catapult design terminology :]
> Although this node will ultimately be invisible by setting the *Z* to `-1`, for now just adjust the *Size* to `(25,25)` so it's easily
> clickable and visible for editing.
>
![Cantilever Node](../Tutorial-Images/xcode_spritekit_add_cantilever_node.png)
>
Ensure this node's physics body is set to *Static*:
>
![Static Body](../Tutorial-Images/xcode_spritekit_static.png)
>
> It's important that this node does not collide with any other physics objects, you wouldn't want the penguin to accidentally hit this invisible node and fall to the ground.  Even if a node is invisible, the physics body is not.
> Set the *Category Mask* and *Collison Mask* to `0`

Next we need to create a code connection for this node.

> [action]
> Add a code connection for the `cantileverNode`
>

Next we need to join these bodies with our *SKPhysicsJointSpring*.

> [action]
> Add the following after the pin join creation.

Now it's time to run the game, your catapultArm should be standing proud, hopefully like this.

![Screenshot cantileverNode](../Tutorial-Images/screenshot_cantilever_node.png)

Notice the faint blue lines showing the connection between our two bodies? This is because we added the physics debug flag when launching the *GameScene*, this is really handy when developing your physics models.

#Pulling the catapult arm

Great, so we have a catapult arm with a spring attached to a static node.  How do we actually pull the spring back to create this force ready to be released upon our penguin?

## The basic Dragging concept

Here's a short outline of what we are going to do:

- If a player starts touching the catapult arm, we create a springJoint between the touchNode and the catapultArm
- Whenever a touch moves, we update the position of the touchNode
- When a touch ends we destroy the joint between the touchNode and the catapultArm so that the catapult snaps and fires the penguin

It's easier to see it in action, there's a lot todo so make sure you pay extra attention :]

##Adding the touchNode

> [action]
The easiest way to create this *touchNode* is to *Copy* & *Paste* the **cantileverNode** and rename it to **touchNode** and place it behind the catapult arm.
>
> ![Adding the touchNode](../Tutorial-Images/xcode_spritekit_add_touch_node.png)
>
> Next you need to code connect the `touchNode`

In case you need to double check your code connections so far in this section here is a quick recap:

> [action]
> Ensure your *GameScene* class properties include:
>
```
var catapultArm: SKSpriteNode!
var catapult: SKSpriteNode!
var cantileverNode: SKSpriteNode!
var touchNode: SKSpriteNode!
```
>
> Check the subsequent connections in `didMoveToView(...)`:
>
```
catapultArm = childNodeWithName("catapultArm") as! SKSpriteNode
catapult = childNodeWithName("catapult") as! SKSpriteNode
cantileverNode = childNodeWithName("cantileverNode") as! SKSpriteNode
touchNode = childNodeWithName("touchNode") as! SKSpriteNode
```
>

##Dynamic joints

Next up we will be dynamically creating a sprint joint whenever the player touches the catapult arm to pull back the catapult.  It needs to be accessible as when the player lets go of the catpault arm we need to destroy the join to release the arm and set it in motion.

> [action]
> Add the following property to the *GameScene* class:
>
```
/* Physics helpers */
var touchJoint: SKPhysicsJointSpring?
```
>

While we are focusing on modeling the catapult physics, we're going to park the penguin launching code for later.

##Touch control

> [action]
> Replace the existing `touchBegan(...)` method with:
>
```
override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
    /* Called when a touch begins */
>    
    /* There will only be one touch as multi touch is not enabled by default */
    for touch in touches {
>        
        /* Grab scene position of touch */
        let location    = touch.locationInNode(self)
>        
        /* Get node reference if we're touching a node */
        let touchedNode = nodeAtPoint(location)
>        
        /* Is it the catapult arm? */
        if touchedNode.name == "catapultArm" {
>            
            /* Reset touch node position */
            touchNode.position = location
>            
            /* Spring joint touch node and catapult arm */
            touchJoint = SKPhysicsJointSpring.jointWithBodyA(touchNode.physicsBody!, bodyB: catapultArm.physicsBody!, anchorA: location, anchorB: location)
            physicsWorld.addJoint(touchJoint!)
>            
        }
    }
}
```
>

When the player touches the screen, we move our **touchNode** to this position and then setup the sprint *SKPhysicsJointSpring* to create this pullback mechanic.

Run the project, the **touchNode** will move to the position the player touches.  However, the player can't pull back the catapult...

##Lean back

To pull the catapult arm we need the **touchNode** to follow the player's touch.  We can do this by adding support for `touchedMoved` method.

> [action]
> Add the following method to *GameScene*
>
```
override func touchesMoved(touches: Set<UITouch>, withEvent event: UIEvent?) {
    /* Called when a touch moved */
>    
    /* There will only be one touch as multi touch is not enabled by default */
    for touch in touches {
>        
        /* Grab scene position of touch and update touchNode position */
        let location       = touch.locationInNode(self)
        touchNode.position = location
>        
    }
}
```
>

Great you can now effectively pull back the catapult, there is one last step required now to launch.  Once the player stops touching the screen we need to sever the **touchJoint**, there by releasing the catapult arm from the **touchNode** and allowing the forced stored in the **cantileverNode** spring joint to now take effect.

##Let it go

> [action]
> Add the following code to *GameScene*
>
```
override func touchesEnded(touches: Set<UITouch>, withEvent event: UIEvent?) {
    /* Called when a touch ended */
>        
    /* Let it fly!, remove joints used in catapult launch */
    if let touchJoint = touchJoint { physicsWorld.removeJoint(touchJoint) }
}
```
>

Run the project, you should now have a working catapult model, a massive improvement on our static catapult launcher :)

![Catapult 2.0](../Tutorial-Images/animated_catapult_2.0.gif)

> [info]
> It's important to run physics based on your device, simulator performance can really affect the physics simulation. So always test on device.
>

#Summary

Great! Now you are really close to completing the shooting mechanism. We've covered a lot of ground here, don't worry if
you don't get it straight away.  The best way to get familiar with game physics is to experiment.

In the next chapter it's time to get those penguins flying.
