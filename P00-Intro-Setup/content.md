---
title: "Intro and Setup"
slug: intro-setup
---
Time for another fantastic tutorial, this time we will be Bowling!

First of all, let’s learn how to get some awesome assets for our bowling game. We can of course use a sphere for our ball, and we can build a lane out of a cube, but what about the pins?

We could use a capsule, but that isn’t really accurate.

We're going to get some free models from the Sketchup 3D Warehouse. You can, of course, use the Unity Asset Store to find models, but if you can't find what you need there, you can look elsewhere. Just be sure to always check licensing information on models you find from sources other than the Unity Asset Store.

> [action]
>Go to the [Sketchup 3D Warehouse](https://3dwarehouse.sketchup.com/model.html?id=9ad3782c82b1e1ba70d6f696167a3190) where we can get a free bowling pin model.

This link has a bowling pin that only has 80 polygons. Always mind the number of polygons as higher means more complex, which is more expensive to render.

![Bowling Pin Download](assets/pin_sketchup.png)

> [action]
>Unity supports Collada files, so click the red Download button and save the Collada file to your computer. Then extract the zip file somewhere so you can find model.dae.

<!-- -->

> [action]
>Now make a new 3D Unity Project called "Bowling."

<!-- -->

> [action]
>Drag the Collada file into your project

![Import Model](assets/drag_file.gif)

Now you should have a model prefab in your assets folder. This is your bowling pin. If you drag it on the Scene View you will see a giant bowling pin rendered to the screen.

![Import Model to Scene](assets/drag_pin.gif)

This bowling pin is too big. In order to understand the scale it should be let’s build our lane.

> [action]
>Delete the pin if you dragged on in, then create a cube and stretch it to be long and skinny, like a bowling lane. We used a scale of (15,0.1,3).

![Create Lane](assets/Capture1.png)

This will create a nice surface to bowl on.

<!-- -->

> [action]
>Now let’s make a material for this bowling lane, we can just use the standard unity shader, set the Albedo color to a brown or gold and drag the material to the cube to assign it.

![Lane Material](assets/Capture2.png)

Now if you drag the bowling pin out again you will notice it is gigantic. You will want to shrink it down by setting it’s scale to something more manageable.

> [action]
>Set it’s scale to 0.025 for all 3 axes, create an Empty Game Object called Pin, and drag this model inside. We do this rather than just scaling down our pin so that we can think of our Pin Prefab as being scaled (1,1,1), which is likely to be convenient later.

![Scale Pin](assets/Capture4.png)

<!-- -->

> [action]Now place this pin at the back of the lane and make a new Prefab for this pin. As a refresher on how to make the Pin a new Prefab, give the pin a name in the Hierarchy panel and drag it into your Project panel.

![Pin Prefab](assets/Capture6.png)

Making a prefab will record the size of the model and make it easier to make changes to entire groups of pins at one time. We are going to make a number of changes, and it will be good to do this to the prefab instead of to a single pin at a time.

Now let’s make the pin respond to physics and collide with things.

> [action]Add a Rigidbody component, and a Box Collider to the pin. You will want to resize the box collider so that it is the same size as the pin.

![Pin Physics](assets/Capture7.png)

<!-- -->

> [info]
>We could have used 3 convex Mesh Colliders (one for each mesh) if we had wanted more accurate collisions, but Mesh Colliders are expensive, and a Box Collider is good enough for what we want to be able to do.

<!-- -->

> [action]
Now duplicate the pin over and over the make the expected pin layout.

![Bowling Setup](assets/Capture8.png)
