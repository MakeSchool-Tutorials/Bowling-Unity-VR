---
title: "Intro and Setup"
slug: intro-setup
---
Time for another fantastic tutorial, this time we will be Bowling!

First of all, let’s learn how to get some awesome assets for our bowling game.  We can of course use a sphere for our ball, and we can build a lane out of a cube, but what about the pins?

We could use a capsule, but that isn’t really accurate.  We're going to get some free models from online.

>[action]
>Go to the [Sketchup 3D Warehouse](https://3dwarehouse.sketchup.com/model.html?id=9ad3782c82b1e1ba70d6f696167a3190) where we can get a free bowling pin model.

This link has a bowling pin that only has 80 polygons. Always mind the number of polygons as higher means more complex, which is more expensive to render.

![Bowling Pin Download](assets/image00.png "Bowling Pin Download")

>[action]
>Unity supports Collada files, so click the red Download button and save the Collada file to your computer.  Then extract the zip file somewhere so you can find model.dae.

<!-- -->

>[action]
>Now make a new 3D Unity Project called "Bowling."

<!-- -->

>[action]
>Drag the Collada file into your project like this:
![Import Model](assets/image01.png "Import Model")

Now you should have a model prefab in your assets folder.  This is your bowling pin. If you drag it on the Scene View you will see a giant bowling pin rendered to the screen:
![Import Model to Scene](assets/image02.png "Import Model to Scene")

This bowling pin is too big.  In order to understand the scale it should be let’s build our lane.

>[action]
>Delete the pin if you dragged on in, then create a cube and stretch it to be long and skinny, like a bowling lane:
![Create Lane](assets/image03.png "Create Lane")
Set the cube's scale to 15,0.1,3
This will create a nice surface to bowl on.

<!-- -->

>[action]
>Now let’s make a material for this bowling lane, we can just use the standard unity shader, set the Albedo color to a brown or gold and drag the material to the cube to assign it:
![Lane Material](assets/image04.png "Lane Material")

Now if you drag the bowling pin out again you will notice it is gigantic.  You will want to shrink it down by setting it’s scale to something more manageable.

>[actio]
>Set it’s scale to 0.025 for all 3 axes:
![Scale Pin](assets/image05.png "Scale Pin")

<!-- -->

>[action]Now place this pin at the back of the lane and make a new Prefab for this pin. As a refresher on how to make the Pin a new Prefab, give the pin a name in the Hierarchy panel and drag it into your Project panel:
![Pin Prefab](assets/image06.png "Pin Prefab")

Making a prefab will record the size of the model and make it easier to make changes to entire groups of pins at one time.  We are going to make a number of changes, and it will be good to do this to the prefab instead of to a single pin at a time.

Now let’s make the pin respond to physics and collide with things.

>[action]Add a RigidBody component, and a Box Collider to the pin.  You will want to resize the box collider so that it is the same size as the pin:
![Pin Physics](assets/image07.png "Pin Physics")

<!-- -->

>[info]
>We could have used 3 convex Mesh Colliders (one for each mesh) if we had wanted more accurate collisions, but Mesh Colliders are expensive, and a Box Collider is good enough for what we want to be able to do.

Now duplicate the pin over and over the make the expected pin layout.
![Bowling Setup](assets/image08.png "Pin Setup")
