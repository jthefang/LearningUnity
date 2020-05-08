# Cinemachine 

- [Cinemachine](#cinemachine)
  - [Setup](#setup)
  - [Set target for camera to follow](#set-target-for-camera-to-follow)
  - [Setup multiple cameras and interpolate between them](#setup-multiple-cameras-and-interpolate-between-them)
  - [Different types of cameras](#different-types-of-cameras)
  - [Cinemachine 2D](#cinemachine-2d)
  - [Track and dolly](#track-and-dolly)

- Based off this [tutorial video](https://www.youtube.com/watch?v=Ml8ptNeezsU)
- Works on top of normal Unity cameras
- There's a `Brain` + `VirtualCamera`s
  - `VirtualCamera`s can be animated/moved around, have them follow GameObjects
  - You can also interpolate between them
  - Brain processes between all that data to create the final "shot"

## Setup

- Package Manager > Cinemachine > Install
- There should be a new `Cinemachine` menu in the top bar
- Add `CinemachineBrain` component to your `Main Camera` game object
- Add new Virtual Camera: Cinemachine menu (top menu bar in Unity) > Create Virtual Camera

## Set target for camera to follow

- Set the `Follow` and `Look At` GameObject fields on your virtual camera
- Expand `Aim` options (for Look At behavior)
  - Dead zone = area within which the GameObject can move without changing the camera view angle
  - Soft zone = area within which the camera will move to look at the GameObject
  - Damping will slow how fast the camera tracks the GameObject
  - Red area = area within which the camera will move to look at the GameObject with no damping
- Expand `Body` options (for Follow behavior)
  - offset = how far the camera will be from the follow target
  - damping will slow how fast the camera follows the target

## Setup multiple cameras and interpolate between them

- Cinemachine brain automatically interpolates between virtual cameras based on their `Priority` field values
- If you increase virtual camera A's priority to infinity (or just make all the other's negative priority) => brain will use A's view
- Default Blend changes interpolation behavior
  - default is `Ease In and Out` with time of 2 seconds

## Different types of cameras

- FreeLook Camera: to move around with the camera
- Clear-Shot Camera: to make sure target is always in view

## Cinemachine 2D

- Cinemachine > Create 2D camera
- Same Follow and Look At fields
- Can also play with priorities to have multiple virtual cameras

## Track and dolly

- [Video tutorial](https://www.youtube.com/watch?v=q1fkx94vHtg)
- Can add a camera dolly with track path => animate camera along track
- Cinemachine > Create Dolly Camera with Track
  - Dolly track = series of waypoints
  - Add a waypoint in the Inspector (just a position)
    - can also position the waypoint via the Gizmo in the scene view
  - Also need a VirtualCamera 
    - it's Body > `Path Position` determines it's interpolation between the waypoints
    - each integer value for this position = one waypoint (0-indexed); float values = interpolation between 2 waypoints
  - Drag VirtualCamera to `Track Timeline` window > Animation track 
    - => animate path the camera takes between the waypoints
- Track Timeline > `Cinemachine track` 
  - allows us to create a series of shots 
  - Right click timeline > Add Cinemachine shot clip > Inspector > Drag a VCam to the Cinemachine Shot slot 
- Can use track and dolly to track a transform
  - Assign transform to VCam `follow` => Enabled Body > `Auto Dolly`
  - Cinemachine will automatically move camera along waypoints to follow transform
