- Following this [tutorial](https://www.youtube.com/watch?v=4HpC--2iowE&ab_channel=Brackeys)

## Cinemachine for the camera

- Install view Package Manager
- Cinemachine > Freelook Camera 
  - Name this the 3rd person camera
  - The Main Camera has a Cinemachine Brain with the Freelook Camera as the live camera => it'll follow the freelook camera
- Set the Player as the Follow + Look At property of the camera
  - Adjust orbits = Halos shown around the camera in the Scene view
  - The camera blends between 3 orbits
    - adjust the height and radius of the orbit rigs (top, middle, bottom) to determine how high and far away the camera is depending on it's position in the orbits
    - you can preview the camera movement by changing the Y Axis and X Axis values in the Axis Control property
    - It normally works well to have camera move close to player as it gets close to the ground
      - e.g. topright: height=14, radius=12
      - middlerig: height=4, radius=18
      - bottomrig: height=-1.5, radius=12
  - Set `Binding mode` of Orbits to `World Space`
    - want character to follow the orientation of the camera and not the other way around
  - Adjust the `Speed` and `Accel time` of the `Y Axis` and `X Axis` of the camera to change how the camera sensitivity works
  - Turn on `Game Window Guides` to adjust the deadzones of the camera
- To prevent Cinemachine from going inside objects with colliders
  - Inpsector > Add Extension (bottom) > CinemachineCollider
  - Check which layer to detect collisions with (label all environment objects with the layer Ground)
  - Ignore tag `Player` to ignore collisions with player (tag the player object with `Player`)
  - Strategy: `Pull Camera forward` 
    - will pull camera in front of the collided obstacle
    - Add damping + damping when occluded (e.g. 0.6, 0.6) to have the camera pull forward smoothly
  - Lower the Lens > near clip plane of the FreeLook to allow the camera to get closer to obstacles without clipping through

### To get cinemachine to work with the new input system

- Add `Cinemachine Input Provider` to the Free Look Camera
- Add the new look action to an `Input Actions`
  - Action Type: `Pass through`, Control Type: `Vector 2`
  - Processors: `Invert Vector 2`
  - Binding Path: Mouse > Delta
  - Use this as the XY axis control Input Action on the `Cinemachine Input Provider` component

## CharacterController

- Add a `CharacterController` to the Player
  - this is essentially a motor to move the Player object + physics collider
  - From Unity: `A CharacterController allows you to easily do movement constrained by collisions without having to deal with a rigidbody. A CharacterController is not affected by forces and will only move when you call the Move function. It will then carry out the movement but be constrained by collisions.` ([reference](https://docs.unity3d.com/ScriptReference/CharacterController.html))
  - Adjust the height of the CharacterController to the player object
- Example `ThirdPersonMovement` script:
```c#
public class ThirdPersonMovement: MonoBehaviour {
  public CharacterController controller; //assign this to the CharacterController

  public Transform cam; //should reference the Main Camera

  public float speed = 6f;

  public float turnSmoothTime = 0.1f;
  float turnSmoothVelocity;

  void Update() {
    float horizontal = Input.GetAxisRaw("Horizontal");
    float vertical = Input.GetAxisRaw("Vertical");
    Vector3 direction = new Vector3(horizontal, 0f, vertical).normalized;

    if (direction.magnitude >= 0.1f) {
      float targetAngle = Mathf.Atan2(direction.x, direction.z) * Mathf.Rad2Deg + cam.eulerAngles.y; // get angle from Z-AXIS (direction player is facing) to direction vector
      float angle = Mathf.SmoothDampAngle(transform.eulerAngles.y, targetAngle, ref turnSmoothVelocity, turnSmoothTime); //smoothly turn
      transform.rotation = Quaternion.Euler(0f, angle, 0f); //rotate to face direction of travel

      Vector3 moveDirection = Quaternion.Euler(0f, targetAngle, 0f) * Vector3.forward;
      controller.Move(moveDirection.normalized * speed * Time.deltaTime); //move in direction
    }
  }
}
```
