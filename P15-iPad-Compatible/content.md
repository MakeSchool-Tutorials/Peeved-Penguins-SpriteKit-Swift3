---
title: Make it iPad compatible
slug: ipad-compatible
---

SpriteKit allows you to adapt your game for the iPad without having to supply new assets and only requires making a few small changes.

Try running your project on an iPad to see what happens...

You will most likely notice the following:

- The main menu is useable, although it looks zoomed in.
- The main game is unplayable.
- It's running in portrait mode :]

Let's fix the orientation issue, we only ever want this game to use Landscape.

> [action]
> Open *info.plist* and expand the options at the bottom.
> You will notice the *iPad orientation* options contain **Portrait** options, remove these.
> ![Penguin animation](../Tutorial-Images/xcode_info_plist.png)

Run the game again, it should be Landscape now and note the issues above.

#Aspect

Currently we are using a scene *scaleMode* or `.AspectFill` which looks great on the iPhone devics.  However, for the iPad we are missing too much of the game out.  Let's modify `.AspectFill` to `.AspectFit`.

> [action]
> Switch to the *Find Navigator* tab and search for `AspectFill`
> ![Search](../Tutorial-Images/xcode_project_search.png)
>
> Replace all finds with `.AspectFit`

Run the game.

Looks good, a bit of letterbox however more than acceptable. Some games simply add a bit of decoration in this space.
Yet the *GameScene* does not look quite right.

![iPad Broken](../Tutorial-Images/screenshot_ipad_broken.png)

So close!

This is an easy fix, to ensure everything is centered correctly we need to change the *GameScene* scene attributes to use an anchor point of `(0.5,0.5)`

Now run the game, looking good :)

#Well done!

You now have a fully functional iPad game!
