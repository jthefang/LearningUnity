
# Table of Contents
- [Unity UI](#unity-ui)
- [Assets](#assets)
- [Materials](#materials)
- [GameObjects](#gameobjects)
	- [Prefabs](#prefab)
	- [Camera object](#camera-object)
	- [Components](#adding-components)
		- [Physics](#physics)
			- [RigidBody](#rigidbody)
	- [Labels](#labels)
	- [GameManager](#gamemanager)
- [Hierarchy](#hierarchy)
- [Scripts](#scripts)
	- [Collision detection](#collision-detection)
	- [Random rotations](#random-rotations)
- [Input Manager](#input-manager)
- [Animation](#animation)
	- [Add simple move left animation](#add-simple-move-left-animation)
	- [Adding sound effects](#adding-sound-effects)
	- [Curves](#curves)
	- [Animation transitions](#animation-transitions)
		- [Parameters and conditions](#parameters-and-conditions)
	- [State machine behaviors](#state-machine-behaviors)
	- [Animation events](#animation-events)
- [Audio](#audio)
	- [Random pitch](#random-pitch)
- [Particle systems](#particle-systems)
	- [Scripting](#scripting)
# Unity UI
- Hand tool - `Q`
	- allows you to pan around the scene
	- `Right click + WASD` to move around in the scene
		- + `Q, E` to go up and down
		- + `Shift` to move faster
- Gizmo = 3d geometry or texture giving info about the GameObject (e.g. wind direction)
	- e.g. coordinate system in the top right
		- click on either axis to switch the scene to look along that axis
		- clicking the middle changes between perspective and isometric/orthographic views
	- `x` = red, `y` = green, `z` = blue 
- Translate tool - `W` 
	- click + drag plane to move object along a plane
- Rotate tool - `E`
	- free rotation - click on object
- Scale tool - `R`
	- scale along 1 axis or proportionally along all 3 (click center of the object and drag)
- Rect tool - `T`
	- for 2D objects
	- resize, rescale, rotate 2D assets + reset pivot point
- Transform tool - `Y`
	- combines translate, rotate and scale tools into 1 
- Center vs Pivot
	- if you select 2 GameObjects, this changes the pivot to be the center between the 2 or completely inside one of the objects
- Global mode vs Local mode
	- toggles coordinate system of the selected GameObject to be that of the global game or of the local object itself
- Click on GameObject --> Hover over scene --> `F` to focus on that object
- 2 - toggles 2D mode in the scene view
- Game play buttons: play, pause, step
	- step through frame by frame
	- pause to make changes
	- NOTE: any changes made during play mode disappear once you exit! This is just to allow you to experiment!
- Window menu to add views to your Layout
	- you can save your custom Layout in the Layout dropdown
- GameManager = normal C# script but is "special" in the sense that it's the brains of the game
- `Alt + Click` expand triangle in file browser - expands entire hierarchy for that item
# Assets
- FBX file = 3D model + animations + textures
	- could be represented as normal jpg --> Fix Now in Unity to add the normal map (?)
- Organization
	- Animations
	- Models - your GameObject models (`.fbx`)
		- the actual GameObject models you want to click and drag into the scene/hierarchy 
		- to view the model without the textures, i.e. the wireframes: Scene view --> Shaded dropdown select --> select wireframe
	- Materials
	- Prefabs
	- Presets
		- In Inspector --> Component --> Presets (2nd top right icon)
		- you can save / load presets (configuration) for a component
	- Scenes
		- where you should store the scenes you create in the hierarchy (click and drag from hierarchy to store scene in Project --> Scenes folder) 
	- Scripts
	- Sounds
	- Textures (`.psd`)
- assets typically packaged into a **unity package**
	- Assets --> Import package --> Custom package --> select the `.unity` file to import the assets package into unity
# Materials
- Determine the look and feel of an object
	- uses a **shader** (simple program written in C-like language for GPU rendering) to give texture
- Project --> Create --> Material
	- preview the material by pulling up the display view at the bottom of the inspector
	- Standard shader gives plenty of flexibility
	- To have an object fade out set **rendering mode: Fade**
	- albedo is the actual texture (drag a texture file to this field)
- Apply the material to a GameObject by dragging it onto the object in the scene
	- This adds a material component to the GameObject
# GameObjects 
- everything in the hierarchy view is a GameObject
- Transform component - controls position, rotation and scale
	- hover over the axis and drag left and right to increment/decrement by small amounts
- `Cmd + D` to duplicate GameObjects in the Hierarchy view
	- multiple GameObjects can share the same name (usually good if they all behave as one entity = a set of columns => also group them under the same parent and name as well)
- Check/uncheck the box next to the GO's name in the inspector to toggle the GO (display/not display)
- Add other components to control behavior and appearance of the GameObject
	- e.g. lighting to illuminate the GameObject
	- e.g. Audio source to accompany the GameObject
- Blue = instance of a model (container of GameObjects) / connected to other objects with a Prefab
	- black = standard GameObject
	- brown = lost prefab connection
## Prefab
- abstraction for a GameObject = underlying "class" that you can use as blueprint for a bunch of GameObject instances
- => changing the Prefab changes all GameObject instances connected to it
- always modify a Prefab instead of the GameObject directly (unless you're experimenting)
- create a prefab by dragging GameObject from hierarchy view to Project folder
- Open prefab (Inspector) to edit --> back button in Hierarchy to return to Scene view
## Camera object
- Projection: **perspective (for 3d games)** = depth, **orthographic (2d)** = no depth perspective
- Clear Flags --> solid color, Background --> black color
	- to get a solid black background
- Camera component
	- field of view = zoom of the camera
- Have camera follow player by:
	- CameraMount GameObject => Add Camera as a child to this object
	- Add a `CameraMovement.cs` script to CameraMount 
		- see script in Bobblehead Wars
	- set Follow Target = player
- Align scene view with camera view by clicking on Scene view → Select Main Camera in Hierarchy → GameObject -> Align View to Selected
## Adding components
- Copy/paste entire component values by clicking the Gear icon (top right)
- Scripts
	- select GameObject in Hierarchy --> Component --> Scripts --> select the script component you want to add (e.g. Ship Controller)
	- Misc:
		- you can define (public) fields in your script that can be accessed in the Inspector Editor 
		- scripts can require other components to be included along with it (e.g. audio source component with an Ion Cannon component to the spaceship GameObject)
	- e.g. Screen Wrap:
		- add the body mesh element to renderers...this allows the game object to wrap around/Pacman teleport when it goes off screen
### Physics
- By default GameObjects are treated as **static colliders** (Unity optimizes it for collision detection, but if you move the object around in the game => engine has to reoptimize) => don't use this option for moving objects
- e.g. select GameObject --> Inspector --> Add component --> Physics --> RigidBody
#### RigidBody 
- if you want the GameObject to be able to have collisions with other objects
- uncheck gravity if you don't want to apply gravity
- check **is kinematic** if the **object will be controlled manually** (rather than just letting the physics engine control it) and you want to **register collisions** (be notified)
	- uncheck kinematic if the object will be given velocity and not controlled (e.g. a bullet)
- **Drag** - friction
- **Angular drag** - damping/friction when rotating
- **Freeze position** - disallows GameObject movement along the checked axes, e.g. checking Y means the GameObject can't move up/down
#### SphereCollider 
- the GameObject's "boundaries"
	- green outline = collision boundary
	- set center to change position of the collider (hitbox for registering a collision)
	- check is trigger to listen for trigger events from other kinematic objects (e.g. a collision)
		- a kinematic GameObject can't register normal collisions
- MeshCollider => check convex
	- To create a collider on a 3D object 
## Labels 
- GameObjects visible in the Scene view, but invisible during gameplay
- Hierarchy → Right click --> Create --> Create empty
	- create label by clicking on the cube in Inspector of the new GameObject --> click the blue capsule
	- this will annotate the GameObject in the scene view (i.e. create a label)
	- change size of this label by clicking Scene View --> Gizmos --> 3D Icons (drag to right to increase size)
- To create spawn point
	- To position it exactly on top (or in same plane) as another object (e.g. for collisions to occur), drag object to be a child of the specific GameObject (not container) you want it to be aligned with => set position to 0, 0, 0 => you can readjust the hierarchy to have the new object be a sibling again instead of a descendant
- Group GameObjects (e.g. marine head and body by **parenting them under an empty GameObject**)
	- **a GameObjects Transform is always relative to its parent**
## GameManager
- This is itself a GameObject 
	- create empty in Hierarchy --> add component --> scripts --> GameManage.c#
- It can have some configurable fields
	- add prefabs, GameObjects (e.g. to let it know what [prefab] to spawn where [GameObj loc])
- Can require an Audio Source component to have game music (check the loop option for this) 
# Hierarchy
- `Cmd + Delete` to delete a GameObject
# Scripts
- script derives from class called `MonoBehaviour`
- Public fields in scripts will be editable in the Unity Inspector
- `Update()`
	- occurs at every single frame (=> no heavy processing)
- `OnEnable()`
	- when GameObject is enabled OR when inactive GameObject reactivates
		- deactivate GameObjects when you don't need it for a while but will need it later
- `Start()`
	- called once in script's lifetime (before Update is called) => do setup/initialization
```c#
public class PlayerController : MonoBehaviour {
    public float moveSpeed = 50.0f; //public variables are editable in Inspector
    
    // Start is called before the first frame update
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {
		//Time.deltaTime = amount of time since last Update call
        Vector3 pos = transform.position; //gets position of current GameObject
        pos.x += moveSpeed * Input.GetAxis("Horizontal") * Time.deltaTime; //Gets horizontal input, updates the position
        pos.z += moveSpeed * Input.GetAxis("Vertical") * Time.deltaTime;
        transform.position = pos;
    }
}
```
- `Destroy()`
	- called right before GameObject is destroyed => clean up (e.g. shut down network connections)
## Collision detection
- When another collider touches this GameObject's collider => it triggers `OnCollisionEnter`
- Add a script to the GameObject and modify `OnCollisionEnter` to control collision behavior
```c#
   void OnCollisionEnter(Collision collision) {
       if (!collided) {
           collided = true;
           GameObject clown = gameObject.transform.parent.gameObject; //current GO's parent GO
           Animator clownAnimator = clown.GetComponent<Animator>();
           clownAnimator.SetTrigger("hit");
       }
   }
```
## Random rotations
```c#
       float randomX = UnityEngine.Random.Range(10f, 100f);
       float randomY = UnityEngine.Random.Range(10f, 100f);
       float randomZ = UnityEngine.Random.Range(10f, 100f);
 
       Rigidbody bomb = GetComponent<Rigidbody>();
       bomb.AddTorque(randomX, randomY, randomZ);
```

# Input Manager
- Edit --> Project Settings --> Input --> Expand axes
- size = number of inputs to the game (default 18 is plenty)
- fields for an input
	- Name = how you'll reference the input in code
	- Descriptive/Negative name = how the input will be read to the user
	- Negative/Positive buttons = actual buttons/keys used for the input (e.g. +: right, -: left)
	- Alt Negative/Positive buttons = alternative buttons (e.g. a, d)
	- other fields mostly for functionality of analog sticks
# Animation
- Mecanim is Unity's animation system
- Window → Animation → Animation (`Cmd + 6)` 
	- To get the animation window
- Select GameObject to animate → Create → Save to create a new animation
	- Adds **animation component** to the GO that points to a `[GO_Name].controller`
	- The **animation clip** allows you to animate several properties of your GO
		- e.g. transform, material color, light intensity, volume of sound, variables in your own scripts
- Animation view
	- Property list = properties to animate
	- The timeline is measure in [seconds]:[frames] (e.g. 3:14 = 3 seconds and 14 frames)
	- Dope sheet = overview of keyframe timing for multiple properties
- Decrease samples to change speed of animation
## Add simple move left animation
- Add property → Transform → Position
- Click on time X you want the movement to occur => change position x to -val
- This adds a **keyframe** that changes the property's value at this particular time
## Adding sound effects
- Click + drag audio file to GO, this adds an Audio Component
	- Disable by unchecking the audio component
- Animation view → Dropdown → Create new clip → name it for your audio → select times on the timeline to activate/deactivate the audio (in inspector)
- NOTE: you will not hear the sound when you play it in the Animation view
## Curves
- The **curves** view allows you to view/modify the exact property values over time 
	- Drag the scrollbar edges to adjust the size of the wave
	- Right click → Add key to add more keyframe points
## Animation transitions
- Using state machines
-  Window → Animation → Animator
	- Entry points to the default state (orange)
	- `Alt + click` to pan
		- + scroll to zoom
- Right click state → make transition → click on the state you want to transition to
	- Unity default blends transitions together
	- To change this
		- Click transition arrow → inspector → settings → Exit time = 1, transition duration = 0
- Transition settings
	- **Has exit time** = whether transition can be triggered at any time or only after specified exit time
	- **Exit time** = earliest time after which transition can start (specified in **normalized time**, e.g. 0.75 = transition starts only when animation is 75% complete)
	- **Transition duration / fixed duration** - specify how long transition takes
		- If fixed duration => specify duration in seconds
		- Else => specify duration in normalized time
	- **Transition offset** - controls offset of time to begin playing destination state (which is transitioned to)
		- e.g. 0.5 = target state will begin playing 50% of way into its own timeline
	- If you want to transition to a state anytime an event X occurs, **regardless of which current state the GO is in, use the **`any` **state**
### Parameters and conditions
- **Condition** used to decide when transition occurs
	- Can have more than 1 condition per transition (=> all conditions must be met to trigger the condition
	- If you want 1 condition OR another condition to trigger a transition => add another transition between the 2 states + another condition with that transition
- **Trigger** = boolean parameter that is rest to false once transition is triggered once
	- To trigger a state:
		- Parameters tab → + → Trigger => Click on transition arrow → Conditions → + 
- **Int** - e.g. count how many times GO is hit => play special animation on the 10th time
	- Can assign a **default value** to this parameter 
## State machine behaviors
- Have GO do something only when it's in a certain state
- Animator view → Click on state → Inspector → Add behavior → new script
	- e.g. make the clown shrink from full size to nothing in 1 second when it enters this state
```c#
float startTime;
 
// OnStateEnter is called when a transition starts and the state machine starts to evaluate this state
override public void OnStateEnter(Animator animator, AnimatorStateInfo stateInfo, int layerIndex)
{
	startTime = Time.time;   
}
 
// OnStateUpdate is called on each Update frame between OnStateEnter and OnStateExit callbacks
override public void OnStateUpdate(Animator animator, AnimatorStateInfo stateInfo, int layerIndex)
{
	GameObject clown = animator.gameObject;
	clown.transform.localScale *= Mathf.Lerp(1, 0, Time.time - startTime);
}
```
## Animation events
- Allows you to call functions from the GO's script when an Animation Clip is running
- Animation View → select moment on the timeline you want the event to happen → Click add event button (top right of the left panel) → inspector → Function dropdown → click the script function you want to trigger
	- e.g. resetting the clown (note that CollisionBehavior is the name of a script on a child GO of the clown GO, collided is a public variable in that script)
```c#
void ResetClownOnHit() {
	gameObject.transform.localScale = Vector3.one;
	gameObject.GetComponentInChildren<CollisionBehavior>().collided = false;
}
```
# Audio
- **AudioSource** component on a GameObject is what will actually play the sound (from an **AudioClip** file) in 2/3D space
	- In 3D space: volume will vary based on how far the AudioSource is from the object listening to it (the **AudioListener** component, which is on the camera/player usually)
		- AudioSource → 3D sound settings → Doppler (doppler effect) and Rolloff (how much volume quiets down the farther its source is from AudioListener, set min and max distance)
		- Closer objects should have more of a linear rolloff
	- In 2D space audio source will play at consistent volume regardless of distance to AudioListener
- Can set up **triggers**, i.e. conditions that cause AudioSources to play
- AudioSource properties:
	- Play on awake: play clip as soon as GO is created
	- Loop: make audio loop
- `.ogg` or `.wav` are lossless formats (preferred over .mp3)
- Scripting
```c#
public AudioClip deathSound; // Drag and drop audio file to this script component in Unity
		...
void OnCollisionEnter(Collision col)
   {
       if (col.gameObject.GetComponent<Animal>() && !col.gameObject.GetComponent<Animal>().isDead)
       {
           Destroy(col.gameObject);
           GetComponent<AudioSource>().PlayOneShot(deathSound);
       }
   }
```
- Add this death sound to the tractor that kills the animals in `OnCollisionEnter` => only have to add it in one place (instead of each individual animal) + animal can't play the sound because it's GameObject is destroyed
- `PlayOneShot` - ensures the sound plays to completion (and won't be cutoff even if `PlayOneShot` is called again)
## Random pitch
`audioSource.pitch = Random.Range(0.8f, 1.5f);`
- Gives more spontaneity to your sounds (e.g. individualistic sheep sounds)
# Particle systems
- Unity uses **Shuriken** particle system to create effects like fire on torches, bomb explosion effects
	- Particle system - emits particles in random positions within predefined space (e.g. a sphere, cone)
	- Particles can have lifetimes after which they are destroyed
- (maybe new) GameObject → Add component → Particle System
	- Particle system component → Renderer section → Material → Default-Particle 
- Particle system component is made of several modules / subsections
- Main module
	- Duration - length of time in seconds for particle system to run (short for explosion, long for fire)
	- Looping - repeatedly emit particles until particle system stops (restarts once duration time is reached) 
	- Prewarm - used when looping enabled, particle system will act as it it's already completed a full cycle on start up (i.e. fill the whole space with particles)
	- Start delay - delay in seconds before particle system starts emitting
	- Start lifetime - initial lifetime in seconds for particles (particles destroyed after this elapsed time)
		- i.e. controls height of flame
	- Start speed - initial speed of particles, greater speed = more spread out/less dense
	- Start size - size of the particles
	- Start rotation - matters if the particles have a distinct shape that changes with rotation
	- Gravity modifier - 0 => gravity will be turned off for the particles (1 = gravity full on)
	- Simulation space - if the particle system moves
		- Local space - particles move together as a unit (if the particle system itself is moved)
		- World space - particles move freely once emitted
	- Play on awake: if disabled => have to manually start particle system via script/animation system
	- Max particles - for performance reasons
- **Emission module** - controls number and timing of emitted particles (continuous flow, sudden burst of particles, etc)
	- Rate over time = # particles / second
		- Set to 0 for a burst of particles (explosion effect)
	- Bursts list - collection of particles emitted all at once at a particular point in time
		- e.g. time = 0.0, particles = 150 will emit 150 particles all at once at start of the system
- Renderer module
	- **Material** - by far the most influential visually; this changes the look of your particles (e.g. how you create a red orange flame)
- Shape
	- Cone
		- Angle determines the cone spread 
		- Radius = how wide the base of the cone is 
	- Can add custom shapes via meshes too
- Size over lifetime
	- Click size box => bottom right curves chart shows x: lifetime of particle, y: particle size
		- Double click to add keys to the graph, right click to remove keys; this changes the shape of the graph
	- There are preset size curves at the bottom of the graph
		- e.g. fire has particles that decrease linearly in size over time
- Color over lifetime
	- Click color box
	- 4 markers, top = alpha/opacity, bottom = RGB values, from start (far left) transitioning to end opacity/color at end (far right)
		- Click the top or bottom bars to add markers, drag marker off bar to delete
	- e.g. to have particles fade away over time => click top right marker and set alpha to 0
## Scripting
- You can have particles emit on certain actions => have script accept a prefab with a particle system










