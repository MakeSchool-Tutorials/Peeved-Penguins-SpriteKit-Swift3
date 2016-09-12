---
title: Polish the Gameplay
slug: improve-gameplay
---

Currently a player can only shoot once. The camera will follow the flying penguin, but won't scroll back to the catapult when the attempt is completed. You are going to improve this behavior.

The rule will be as follows:

If a penguin has stopped or is moving slowly you will remove the penguin and move the camera back to the catapult and reset the catapult mechanic, ready for the next penguin.

You will have to check if this condition becomes *true* on a regular basis.

> [challenge]
> Where do you think this check should take place?

# Next turn

Along with this check, there is no way yet for the player to launch another penguin.  So you will need to implement a way to reposition the camera back to the catapult and also reset the catapult itself.  After you launched the penguin the catapult arm will still be moving back and forth.

> [action]
> Add the following  after `lastTrackerPosition = trackerNode.position` in `update(...)`:
>
```
/* Has penguin come to a near stand still */
let idleVelocity:CGFloat = 0.15
>
/* Is the penguin currently joined to the catapult */
let nodeJoints = trackerNode.physicsBody?.joints.count
>
if trackerNode.physicsBody!.velocity.length() < idleVelocity &&
    nodeJoints == 0 {
>
    /* Reset tracker node */
    self.trackerNode = nil
>    
    /* Move camera back to start position */
    let resetCamera = SKAction.moveTo(x: 0, duration: 1.0)
    camera.run(resetCamera)
>    
    /* Reset catapult arm */
    catapultArm.physicsBody?.velocity = CGVector(dx:0,dy:0)
    catapultArm.physicsBody?.angularVelocity = 0.0
    catapultArm.zRotation = 0
    catapultArm.position = CGPoint(x:-81,y:31)
>    
    /* Remove penguin */
    let removeNode = SKAction.removeFromParent()
    trackerNode.run(removeNode)
}
```
>

While this may look a little complicated at first, what actually is going on is only a tiny bit of math. You want to check whether the **speed** of the penguin has come to reset (or near rest).
The *length* function that calculates the square length of the velocity (basically the x and y component of the speed combined).

There is an issue with this check, it works well once the penguin has been launched.  However, when the penguin is being pulled back on the catapult it will have a **speed** or *length* less than our *idleVelocity* value.

You can use `cameraTarget.physicsBody?.joints.count` to check if the penguin is attached to the catapult arm.
The **trackerNode** is a refernce to the penguin and while the penguin it is attached to the catapultArm there will be a joint attached to it, giving the **joints.count** a value of `1`.

The camera can be reset nicely using an *SKAction* to return the camera position back to the starting point of `(0,0)`.

It's a little tricker to reset the catapult, first you need to remove the forces that are being applied to it from the launch.  Then you need to reset both the rotation and position back to the original values, you can look in *GameScene.sks* to check these values.

You then remove the penguin from the game scene and the player will now be ready to take their next turn.

# Camera improvements

This is looking great.  However, the camera doesn't quite feel right, if the penguin goes too far to the left or the right the camera will move outside of the game world.  

You can solve this by restricting when the camera action can run by checking the `x` value of the camera's position.  You don't want it to move less than it's starting position of `0` or greater than `392`.  I took the value of the background width `960` and subtracted the wide of the camera `568` to get this value.

> [action]
> Wrap your camera movement code in `update` to look like this:
>
```
/* Create a move action for the camera */
if camera.position.x + moveDistance >= 0 && camera.position.x + moveDistance <= 392 {
    let moveCamera = SKAction.moveBy(x: moveDistance, y: 0, duration: moveDuration)
    camera.run(moveCamera)
}
```
>


# Summary

You've learnt the importance of making those **little touches**, it's often the small things when added all together can really take your game play to the next level and make the experience more memorable for the player.

Always look for ways to improve your game mechanic and make it feel as satisfying as possible for the player.  Get feedback from those around you, make changes and get more feedback.

In the next chapter you will learn how to add iPad compatibility.
