- Following this [tutorial](https://www.youtube.com/watch?v=Pzd8NhcRzVo&ab_channel=Brackeys)
- Package Manager > Input System

## Getting started

- Right click Project > Create > Input Actions
- Double click it
- Action maps = categories for control
  - e.g. one for your Player, one for the main menu, one for vehicles, etc
- Actions = events (e.g. Shoot, bring up new menu)
  - can create bindings = trigger for the action
- Create a new binding on action > Properties
  - Binding = key that triggers the action (e.g. search for `space`)
  - Add another binding to bind multiple keys to this action
  - Interactions > `Press` if you only want the action to trigger on the key press (defaults to press and release)
- Create composite axis on action > Properties
  - Axis has 2 bindings: positive and negative, which you can bind to a key
  - This is for actions that move along and axis
- Create compositive d pad on action > Properties
  - Has 4 directions (up, down, left, right) for actions that have 4 cardinal directions
  - Can bind these to the up, down, left, right or W, A, S, D keys respectively

## Control schemes

- Control schemes are different controller types (e.g. keyboard/mouse, Xbox controller, joystick, etc.)
- Top left dropdown > Add Control Scheme
  - Scheme name: e.g. Keyboard and Mouse
  - Add device for control scheme (e.g. keyboard, mouse)
- For each of your action bindings select which control scheme they apply to

## For game controllers

- You can adjust the sensitivity and other settings via the Processors property on the binding

## Integrating with code

- Check `Generate C# class` in the inspector for the Input Actions object
  - This generates a script for your input actions
  - You can reference this in your code
- e.g. if you named your Input Actions `InputMaster`:
```c#
using UnityEngine.InputSystem;

public class Player: MonoBehaviour {
    public InputMaster controls;

    void Awake() {
        //controls.<Action-Map-name>.<Action-name>.performed (if the action is triggered) is an event
        controls.Player.Shoot.performed += ctx => Shoot(); //add delegate
        controls.Player.Move.performed += ctx => Move(ctx.ReadValue<Vector2>()); //for axis values
    }

    void Move(Vector2 direction) {
        Debug.Log("Move: " + direction);
    }

    void Shoot() {
        Debug.Log("Shot");
    }

    void OnEnable() {
        controls.Enable(); //you have to enable the controls
    }

    void OnDisable() {
        controls.Disable();
    }
}
```

## Check for simple key press

```c#
using UnityEngine.InputSystem;

...
void Update() {
    Keyboard kb = InputSystem.GetDevice<Keyboard>();
    if (kb.spaceKey.wasPressedThisFrame) {
        ...do something
    }
}
```
