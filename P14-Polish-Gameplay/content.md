---
title: Polish the Gameplay
slug: improve-gameplay
---

Currently a player can only shoot once. The camera will follow the flying penguin, but won't scroll back to the catapult when the attempt is completed. We will change that in this chapter. The rule will be as follows: If a penguin has stopped or is moving slowly we will remove the penguin and move the camera back to the catapult and reset the catapult arm itself.

We will have to check if this condition becomes *true* on a regular basis - we can use the update method to do so.

#Implementing the update method

> [action]
> Add the following code after camera clamp in `update(...)`
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

While this may look a little complicated at first, what actually is going on is only a tiny bit of math. We check whether the speed is below our defined limit. Therefore we use the *length* function that calculates the square length of our velocity (basically the x- and y-component of the speed combined). The value of `0.18` was through testing, tweak to your own liking.

This solution is a little bit fancy as we are using `cameraTarget.physicsBody?.joints.count` to check if the penguin has been fired.
The **cameraTarget** is the penguin and while it is attached to the catapultArm with a joint it will have a **joint.count** of 1.  This is a quick way to check this, another way would be to use another property to track the fire state of the penguin.

> [challenge]
> I omitted another scenario here, what if the penguin somehow leaves the borders of the game scene? How would you check for this?

#Reseting the camera on next launch

You may have noticed a problem here, what if the player tries to launch another penguin before the camera has scrolled all the way back.
Well not a major issue however be nice to stop the camera moving.

This can be corrected by removing the *moveTo* action from the camera, let's do this when a new penguin is attached to the catapult.

> [action]
> Add the following code to `touchesBegan(...)`
> Just before:
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

Let's get rid of the red nodes, an easy way to hide them visibly is to modify the *Z* to `-2` e.g. behind the background.  The visibility of a physics node has no effect on the physics simulation.

#Summary

You've learnt the importance of those 'finishing touches', it's often the little tweaks that take your game play to the next level.
Always look for ways to improve your game mechanic and make it feel as satisfying as possible for the player.

You are ready to move on to the next chapter where you will learn how you can make this game iPad compatible!
