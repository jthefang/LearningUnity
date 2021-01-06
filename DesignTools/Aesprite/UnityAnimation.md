- [Simple 2D animation](#simple-2d-animation)
- [Unity's animator](#unitys-animator)
	- [Add simple move left animation](#add-simple-move-left-animation)
	- [Adding sound effects](#adding-sound-effects)
	- [Curves](#curves)
	- [Animation transitions](#animation-transitions)
		- [Parameters and conditions](#parameters-and-conditions)
	- [State machine behaviors](#state-machine-behaviors)
	- [Animation events](#animation-events)

# Simple 2D animation
Based on Brackeys: https://www.youtube.com/watch?v=hkaysu1Z-N8

- Directory organization:
    - `Assets/Animation`
         - `<Sprite>_<Animation>.anim` (e.g. `Player_Run`, `Player_Idle`)
- Scene view > Select sprite to animate > Open Animation view (Window > Animation > Animation)
    - In the Animation window: Create > New .anim
    - Drag all sprite frames (each an individual `.png`) into animation window 
        - this adds each image as a key frame 
- Each animation is associated with an animator
    - Double click the animator controller in the `Assets/Animation/<Sprite>.controller` to open it in the Animator window
    - Drag .anim's into the animator state diagram if needed 
    - What the `entry` state points to is the default animation
        - Right click anim state > Set as default animation to change this
- To make transitions between states:
    - Right click start state > Make transition > Click end state
    - Click on transition (arrow)
        - Add a condition for the transition 
        - Add a parameter for the animator: left panel > Parameters tab > + 
            - e.g. For a transition between an idle state -> run state: add a `float` parameter called `Speed` => add condition `Speed` `Greater` than `0`
    - If you want the transition to be immediate:
        - Uncheck `Has Exit time` 
        - Settings > transition duration = 0
- To make a transition to an animation FROM any state:
    - use the `Any state` state as the start state of the transition
    - Careful about this transition! Settings > `Can Transition to Self` to prevent the state from transitioning to itself over and over (and thus never playing more than the 1st frame of the animation state)
    - e.g. To transition from any state -> jump state: add a `bool` parameter called `IsJumping` => add condition `IsJumping` `true`
        - => Add a transition from jump state -> idle state (if `IsJumping` `false` and `Speed` `Less` `0.01`)
        - => Add a transition from jump state -> run state (if `IsJumping` `false` and `Speed` `Greater` `0.01`)
- Script
```c#
public class SpriteClass {
    Animator animator = GetComponent<Animator>();
    ...
    animator.SetFloat("Speed", some_value);
    animator.SetBool("IsJumping", true);
}
```

# Unity's animator
- Mecanim is Unity's animation system
- Window → Animation → Animation (`Cmd + 6)` 
	- To get the animation window
- Select GameObject to animate → Create → Save to create a new animation
	- Adds **animator component** to the GO that points to an animator controller `[GO_Name].controller`
	- The **animation clip** allows you to animate several properties of your GO
		- e.g. transform, material color, light intensity, volume of sound, variables in your own scripts
- Animation view
	- Property list = properties to animate
	- The timeline is measure in [seconds]:[frames] (e.g. 3:14 = 3 seconds and 14 frames)
	- Dope sheet = overview of keyframe timing for multiple properties
- Decrease samples to change speed of animation
- The controller `[GO_name].controller` contains the option to **loop** the animation (on by default)
## Add simple move left animation
- Add property → Transform → Position
- Click on time X you want the movement to occur => change position x to -val
- This adds a **keyframe** that changes the property's value at this particular time
- Another way to do it is to click the red record button --> click the key time on the timeline --> change transform position X in the inspector --> stop recording by clicking record button again
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
- You can **copy and paste** animation states
	- set speed to -1 to **reverse animation**
- Right click state --> **Set as Layer default state** to have this state be the entry animation/state
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
- **Trigger** = boolean parameter that is set to false once transition is triggered once
	- To trigger a state:
		- Parameters tab → + → Trigger => Click on transition arrow → Conditions → + 
- **Int** - e.g. count how many times GO is hit => play special animation on the 10th time
	- Can assign a **default value** to this parameter 
- **Bool** - add as parameter in Animator --> click on a transition and add condition bool = T/F
	- this makes it so that whenever the bool is T/F the transition is triggered
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