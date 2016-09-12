---
title: Make it iPad compatible
slug: ipad-compatible
---

SpriteKit allows you to adapt your game for the iPad without having to supply new assets and only requires making a few small tweaks.

# iPad tweaks

Try running your game on an iPad to see what happens...

You will notice the following:

- The game view is zoomed in
- It's unplayable
- It's running in portrait mode.

Let's fix the orientation issue first, as this game was designed for landscape.

> [action]
> Open *info.plist* and expand the list at the bottom.
> Notice the *iPad orientation* values, remove any **Portrait** options.
>
> ![Xcode plist](../Tutorial-Images/xcode_info_plist.png)

Run your game... It should now run in landscape mode.

## Scale mode

Currently you are using a scene *scaleMode* or `.aspectFill` which looks great on the iPhone.  However, for the iPad you can't see all of the game content.  Let's try another scale mode, `.aspectFit` will stretch the view to fit the device while using the aspect ratio of the device.

> [action]
> Switch to the *Find Navigator* tab and search for `aspectFill`
>
> ![Search](../Tutorial-Images/xcode_project_search.png)
>
> Replace all results of `.aspectFill` with `.aspectFit`
>

Run your game...

Looking better, the only side effect is the letterbox effect.  However, I feel this is more than acceptable for the trivial amount of work required to achieve this compatibility.

# Summary

Congrats on finishing *Peeved Penguins*, give yourself or the person next to you a high five.

The next chapter will be a recap of everything you have covered so far, well done.
