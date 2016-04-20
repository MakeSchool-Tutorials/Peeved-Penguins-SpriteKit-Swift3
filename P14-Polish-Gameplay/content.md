---
title: Polish the Gameplay
slug: improve-gameplay
---

Currently a player can only shoot once. The camera will follow the flying penguin, but won't scroll back to the catapult when the attempt is completed. You are going to that in this chapter.

The rule will be as follows:

If a penguin has stopped or is moving slowly we will remove the penguin and move the camera back to the catapult and reset the catapult arm itself.

You will have to check if this condition becomes *true* on a regular basis, where do you think this check should be made?

#Implementing the check

> [action]
> Add the following code after the camera clamp in `update(...)`
```
/* Check penguin has come to rest */
if cameraTarget.physicsBody?.joints.count == 0 && cameraTarget.physicsBody?.velocity.length() < 0.18 {
>
    cameraTarget.removeFromParent()
>
    /* Reset catapult arm */
    catapultArm.physicsBody?.velocity = CGVector(dx:0, dy:0)
    catapultArm.physicsBody?.angularVelocity = 0
    catapultArm.zRotation = 0
>
    /* Reset camera */
    let cameraReset = SKAction.moveTo(CGPoint(x:284, y:camera!.position.y), duration: 1.5)
    let cameraDelay = SKAction.waitForDuration(0.5)
    let cameraSequence = SKAction.sequence([cameraDelay,cameraReset])
>
    camera?.runAction(cameraSequence)
}
```

While this may look a little complicated at first, what actually is going on is only a tiny bit of math. You want to check whether the **speed** of the penguin has come to reset (or near rest).
The *length* function that calculates the square length of the velocity (basically the x and y component of the speed combined).

There is an issue with this check, it works well once the penguin has been launched.  However, when the penguin is being pulled back on the catapult it will have a **speed** or *length* less than the limit of `0.18`.

You can use `cameraTarget.physicsBody?.joints.count` to check if the penguin has been fired.
The **cameraTarget** will point to the penguin and while the penguin it is attached to the catapultArm there will be a joint attached to it, giving the  **joint.count** a value of `1`.  This is a quick way to check this, another slightly more involved way would be to use another property to track the *State* of the penguin. e.g. `Loading, Launched`

> [challenge]
> I omitted another scenario here, what if the penguin somehow leaves the borders of the game scene? How would you check if the penguins position was out of the *GameScene*

#Reset the camera

You may have noticed another slight Ux issue here, what if the player tries to launch another penguin before the camera has scrolled all the way back.?

This can be corrected by stopping the *moveTo* action that's being applied to the camera, let's do this when a new penguin becomes attached to the catapult.

> [action]
> Add the following code to `touchesBegan(...)`, just before:
>
```
/* Set camera to follow penguin */
cameraTarget = penguin.avatar
```
>
```
/* Remove any camera actions */
camera?.removeAllActions()
```
>

##Final tweak

Let's get rid of the red box static nodes, an easy way to hide them using the editor is to modify the *Z-Position* to `-2` e.g. behind the background.  The visibility of a physics node has no effect on the physics simulation. Otherways to achieve this could be setting the sprite to have an *alpha* of `0` or set *hidden* to be `true`.

#Summary

You've learnt the importance of making those **little touches**, it's often the small things when added together that can really take your game play to the next level and make the experience more memorable for the player.

Always look for ways to improve your game mechanic and make it feel as satisfying as possible for the player.
Get feedback and iterate.

In the next chapter where you will learn how you can make your game iPad compatible!
