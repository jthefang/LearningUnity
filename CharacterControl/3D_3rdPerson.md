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
  - Set `Binding mode` of Orbits to `World Space`
    - want character to follow the orientation of the camera and not the other way around
  - Adjust the `Speed` and `Accel time` of the `Y Axis` and `X Axis` of the camera to change how the camera sensitivity works
  - Turn on `Game Window Guides` to adjust the deadzones of the camera
