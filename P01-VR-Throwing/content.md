---
title: "SteamVR"
slug: vr-throwing
---

> [action]
>
Create a sphere and place it at the base of your lane. This will be our ball.

We will get back to this in a moment.

Now we are going to need to import SteamVR into the project so that we can get the hand controllers and control our HMD.

> [action]
>
Open the Asset Store then search for SteamVR and Download and import it.
>
![Download SteamVR](assets/Capture9.png)
>
Once it is done downloading,import it into your project, and then find the SteamVR folder under Assets. Drag a `CameraRig` component to the scene.
>
![Camera Rig in scene](assets/Capture11.png)

The glowing blue box will be your walking play area. You will be able to walk around this region and interact with objects inside of it. For now we will only have the ball in our region of view. We will also want to make the lane slightly shorter, so adjust it’s `X` scale to `8` instead of `15`, this will put us closer to the pins.

Arrange the pins, camera rig, and ball appropriately.

> [info]
>
You can move multiple objects as a group by holding `control` and selecting each one from the Hierarchy before moving them.

# Throwing the Ball

Now we can make the ball grab-able and add physics to it.

> [action]
>
Attach a Rigidbody to the sphere and name it `Ball`. Make this into a Prefab as well.

If we go into the `CameraRig` object in the Scene, you will see that it has an Object for the left controller and the right controller. You can expand these further to see the Models that are attached to each controller, but that we can't see them until we run the Scene. There is a way to change that, and we'll want to do that when we add Colliders.

> [action]
>
Select the each controller's model, and change the Model Override to `oculus_cv1_controller_right` or `left` as appropriate -- this will allow us to see the physical model in the Editor. If you're not presented this option in the dropdown menu, it's probably because your Oculus is not connected or detected. These options appears when the Oculus is detected.
>
![The model appears](assets/Capture105.png)
>
Add a box collider to this Model so that we can use it to collide with our ball. Make sure that `Is Trigger` is checked on the box collider. `Is Trigger` will ensure that when our hand collides with the ball it wont knock the ball around, it will only fire off a `Trigger Enter` so that we can decide what to do.
>
![Hand Collider](assets/Capture10c.png)
>
Now create a new component called `GrabBall` with the following contents, to allow you to pick up and throw the ball:
>
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
>
public class GrabBall : MonoBehaviour
{
    private SteamVR_TrackedObject controller;  //Reference to the controller
    private GameObject selectedObject;
    private GameObject grabbedObject;
>
    void Start()
    {
        controller = GetComponent<SteamVR_TrackedObject>();
    }
>
    void OnTriggerEnter(Collider other)
    {
        selectedObject = other.gameObject; //Select an object by touching it
    }
>
    void OnTriggerExit(Collider other)
    {
        if (selectedObject == other.gameObject)
        {
            selectedObject = null; //Deselect the last object you touched
        }
    }
>
    void FixedUpdate()
    {
        //Get a reference to the steam controller we drag to the script:
        SteamVR_Controller.Device device = SteamVR_Controller.Input((int)controller.index);
        //If we have a grabbedObject selected then align it with the transform.position.
        if (grabbedObject != null)
        {
            grabbedObject.transform.position = this.gameObject.transform.position;
        }
        //If you press the trigger down while an object is selected and not grabbed,
        //Mark the object as grabbed and move it to the hand.
        if ((selectedObject != null && grabbedObject == null) && device.GetHairTriggerDown())
        {    
            grabbedObject = selectedObject;
            grabbedObject.transform.position = this.gameObject.transform.position;
            selectedObject.GetComponent<Rigidbody>().isKinematic = true;
        }
        else if (grabbedObject != null && device.GetHairTriggerUp())
        {
            //If we let go of the trigger while an object is grabbed then apply physics
            var rigidbody = grabbedObject.GetComponent<Rigidbody>();
            rigidbody.isKinematic = false;
            //Apply the current device velocity to the object to throw it, we multiply 5 to make it
            //Easier to throw.
            rigidbody.velocity = grabbedObject.transform.TransformVector(device.velocity * 5);
>
            //Apply the angular velocity from the device as well, also multiply by 5.
            rigidbody.angularVelocity = grabbedObject.transform.TransformVector(device.angularVelocity * 5);
            rigidbody.maxAngularVelocity = rigidbody.angularVelocity.magnitude;
>
            //Now that the object has been thrown lets reset selection and grabbedObject so that we can select a new object!
            selectedObject = null;
            grabbedObject = null;
        }
    }
}
```
>
Now add the component to each hand.

# Increasing mass

Now let’s increase the ball's mass it will knock the pins around.

> [action]
>
Click the Ball GameObject and increase its mass to `5`.
>
![Increase Mass](assets/Capture12.png)

Now if we go into the game we should be able to walk up to the ball, grab it with our hands and throw it at the pins! If it hits the pins it will knock them around! Super cool huh?

This is far from a complete bowling game, now we will need to add scoring, turns, pin reset, and ball reset.

# Challenges

If you're up for a challenge, try adding the following... Brainstorm with someone next to you before you get started!

> [challenge]
>
Fine tune the physics so you can know some pins over! You may want to expose the magic numbers in `GrabBall`... ;)

<!--  -->

> [challenge]
>
Add some gutters so the game is a bit easier.

<!--  -->

> [challenge]
>
Add a realistic texture for the floor and Skybox

<!--  -->

> [challenge]
>
Add scoring into the game!

<!--  -->

> [challenge]
>
Make it so the pins reset after two throws like in a real bowling alley.
