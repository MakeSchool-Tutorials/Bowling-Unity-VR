---
title: "SteamVR"
slug: vr-throwing
---

> [action]
>Now create a sphere and place it at the base of your lane. This will be our ball.

We will get back to this in a moment.

Now we are going to need to import SteamVR into the project so that we can get the hand controllers and control our HMD.

> [action]
>Open the Asset Store then search for SteamVR and Download and import it.

![Download SteamVR](assets/Capture9.png)

<!-- -->

> [action]
>Once it is done downloading, please import it into your project, and then find the SteamVR folder under Assets. Drag a [CameraRig] component to the scene.

![Camera Rig in scene](assets/Capture11.png)

The glowing blue box will be your walking play area. You will be able to walk around this region and interact with objects inside of it. For now we will only have the ball in our region of view. We will also want to make the lane slightly shorter, so adjust it’s X scale to 8 instead of 15, this will put us closer to the pins.

#Throwing the Ball

Now we can make the ball grab-able and add physics to it.

> [action]
>Attach a Rigidbody to the sphere and name it Ball. Make this into a Prefab as well.

If we go into the [CameraRig] object in the Scene, you will see that it has an Object for the left controller and the right controller. You can expand these further to see the Models that are attached to each controller, but that we can't see them until we run the Scene. There is a way to change that, and we'll want to do that when we add Colliders.

> [action]
Select the Model, and change the Model Override to "vr_controller_vive_1_5" -- this will allow us to see the physical model in the Editor. If you're not presented this option in the dropdown menu, it is likely because your Vive is not connected or detected. These options only appear when the Vive is detected.

![The model appears](assets/Capture105.png)

> [action]
Add a box collider to this Model so that we can use it to collide with our ball. Make sure that Is Trigger is checked on the box collider. Is Trigger will ensure that when our hand collides with the ball it wont knock the ball around, it will only fire off a Trigger Enter so that we can decide what to do.

![Hand Collider](assets/Capture10c.png)

> [action]
Now create a new component called Grab Ball with the following contents, to allow you to pick up and throw the ball:
>
```
public class GrabBall : MonoBehaviour {
    private SteamVR_TrackedObject controller;  //Reference to the controller
    private GameObject selectedObject;
    private GameObject grabbedObject;
>
    void Start ()
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
       if(selectedObject == other.gameObject)
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
        if ((selectedObject != null && grabbedObject == null) && device.GetTouchDown(SteamVR_Controller.ButtonMask.Trigger))
        {
            grabbedObject = selectedObject;
            grabbedObject.transform.position = this.gameObject.transform.position;
        }
        else if (grabbedObject != null && device.GetTouchUp(SteamVR_Controller.ButtonMask.Trigger))
        {
            //If we let go of the trigger while an object is grabbed then apply physics
            var rigidbody = grabbedObject.GetComponent<Rigidbody>();
            //Apply the current device velocity to the object to throw it, we multiply 4 to make it
            //Easier to throw.
            rigidbody.velocity = grabbedObject.transform.TransformVector(device.velocity*4);
>
            //Apply the angular velocity from the device as well, also multiply by 4.
            rigidbody.angularVelocity = grabbedObject.transform.TransformVector(device.angularVelocity*4);
            rigidbody.maxAngularVelocity = rigidbody.angularVelocity.magnitude;
>
            //Now that the object has been thrown lets reset selection and grabbedObject so that we can select a new object!
            selectedObject = null;
            grabbedObject = null;
        }
    }
}
```

<!-- -->

> [action]
>Now add the component to each hand.

<!-- -->

Now let’s make the ball much higher mass than the pins so that it will knock them around.

> [action]
Click the Ball GameObject and increase its mass to 5.

![Increase Mass](assets/Capture12.png)

Now if we go into the game we should be able to walk up to the ball, grab it with our hands and throw it at the pins!

If it hits the pins it will knock them over!  Super cool huh?

This is far from a complete bowling game, now we will need to add scoring, turns, pin reset, and ball reset.

If you're up for a challenge, try adding the following:

- scoring
- turns with pin and ball resets

To complete the game, you can make some simpler additions, like adding gutters on the edges, and a more realistic texture for the floor and Skybox.
