# Unit testing
Based off of this [video](https://www.youtube.com/watch?v=r7VkbV0PRC8).

## Humble Object Pattern
We want to separate out testable functionality from our `MonoBehaviour` component, so that we can more easily test it (hard to test functionality on `MonoBehaviour`'s).

Non humble code:
```c#
public class Bird : MonoBehaviour {
    private int maxHeight = 3;
    private int minHeight = -3;

    private void Update() {
        float vertical = Input.GetAxis("Vertical");
        Move(vertical);
    }

    private void Move(float vertical) {
        transform.position += Vector3.up * vertical;
        if (transform.position.y > maxHeight) {
            transform.position = new Vector3(transform.position.x, maxHeight, transform.position.z);
        } else if (transform.position.y < minHeight) {
            transform.position = new Vector3(transform.position.x, minHeight, transform.position.z);
        }
    }
}
```

Humble code:
```c#
public interface IBird { //IBird.cs
    Vector3 Position { get; set; } 
    float MaxHeight { get; }
    float MinWeight { get; }
}
```

```c#
public class BirdSplit : MonoBehaviour, IBird { //BirdSplit.cs
    private BirdController _birdController;

    private float maxHeight = 3;
    public float MaxHeight { get { return maxHeight; } }
    private float minHeight = -3;
    public float MinHeight { get { return minHeight; } }
    public Vector3 Position { get { return transform.position; } set {transform.position = value; } }

    private void Awake() {
        _birdController = new BirdController(this);
    }

    private void Update() {
        float vertical = Input.GetAxis("Vertical");
        _birdController.Move(vertical);
    }
}
```

```c#
public class BirdController { //BirdController.cs
    //this code is testable!
    private IBird _bird;

    public BirdController(IBird bird) {
        _bird = bird;
    }

    public void Move(float vertical) {
        transform.position += Vector3.up * vertical;
        if (_bird.Position.y > _bird.MaxHeight) {
            _bird.Position = new Vector3(_bird.Position.x, _bird.MaxHeight, _bird.Position.z); //setter and getter for Position
        } else if (_bird.Position.y < _bird.MinHeight) {
            _bird.Position = new Vector3(_bird.Position.x, _bird.MinHeight, _bird.Position.z);
        }
    }
}
```

```c#
public class MockBird : IBird {
    public Vector3 Position { get; set; } 
    public float MaxHeight { get; }
    public float MinWeight { get; }
}
```

```c#
using NUnit.Framework;
public class BirdTests { //BirdTests.cs (should be in an Editor/ folder)
    [Test]
    public void BirdStopsAtMinHeight() {
        IBird bird = new MockBird() { MaxHeight = 3, MinHeight = -3 };
        BirdController birdController = new BirdController(bird);
        birdController.Move(-10f);
        Assert.AreEqual(-3f, bird.Position.y);
    }

    [Test]
    public void BirdStopsAtMaxHeight() {
        IBird bird = new MockBird() { MaxHeight = 3, MinHeight = -3 };
        BirdController birdController = new BirdController(bird);
        birdController.Move(10f);
        Assert.AreEqual(3f, bird.Position.y);
    }
}
```

## Code
You should put test scripts in `Tests/Editor` folder.

We can't instantiate objects of type `Monobehaviour` in our test script. Therefore we should use the humble object pattern or use mocks.

### Mocking classes
For when it's impractical (or too lengthy) to humble object a class for testing. Define an interface. And we're going to use `NSubstitute` to do our mocking. 

Create a `Plugins/` Assets folder and drag `NSubstitute.dll` into it (get NSubstitute version 2.0.3 for .NET support).

The class we want to mock:
```c#
// Assume it's impractical to humble object this class => mock this class
public class CharacterMover : MonoBehaviour {
    private CharacterController characterController;
    [SerializeField]
    private bool isPlayer;
    public bool IsPlayer => isPlayer; //can't set this field publicly, but can do so in the inspector
    public int Health { get; set; }

    private void Awake() {
        characterController = GetComponent<CharacterController>();
    }

    private void Update() {
        float horizontal = Input.GetAxis("Horizontal");
        float vertical = Input.GetAxis("Vertical");
        characterController.Move(new Vector3(horizontal, 0, vertical));
    }
}
```

The mocked class
```c#
public interface ICharacterMover {
    int Health { get; set; } //every field in interface is public
    bool IsPlayer { get; }
}

public class CharacterMover : MonoBehaviour, ICharacterMover {
    //... same as above
}
```

The humble object class:
```c#
public class TrapBehaviour : MonoBehaviour {
    [SerializeField] 
    private TrapTargetType trapType;

    private Trap trap;

    private void Awake() {
        trap = new Trap();
    }

    private void OnTriggerEnter(Collider other) {
        var characterMover = other.GetComponent<ICharacterMover>();
        trap.HandleCharacterEntered(characterMover, trapType);  
    }
}

public class Trap { //not a MonoBehaviour => can call new Trap()
    public void HandleCharacterEntered(ICharacterMover characterMover, TrapTargetType trapType) {
        if (characterMover.IsPlayer) {
            if (trapType == TrapTargetType.Player) 
                characterMover.Health--;
        } else {
            if (trapType == TrapTargetType.NPC) 
                characterMover.Health--;
        }
    }
}

public enum TrapTargetType { Player, NPC }
```

The test class:
```c#
using NUnit.Framework;
using NSubstitute;

public class TrapTests {
    [Test]
    public void PlayerEnteringPlayerTargetedTrap_ReducesHealthByOne() {
        Trap trap = new Trap(); //Trap cannot be of type MonoBehaviour
        ICharacterMover characterMover = Substitute.For<ICharacterMover>(); //Mock character mover!
        characterMover.IsPlayer.Returns(true); //mock doesn't implement the interface, but you can make it pretend to return some value
        
        trap.HandleCharacterEntered(characterMover, TrapTargetType.Player);
        Assert.AreEqual(-1, characterMover.Health);
    }

    [Test]
    public void NPCEnteringNPCTargetedTrap_ReducesHealthByOne() {
        Trap trap = new Trap(); //Trap cannot be of type MonoBehaviour
        ICharacterMover characterMover = Substitute.For<ICharacterMover>(); //Mock character mover!
        trap.HandleCharacterEntered(characterMover, TrapTargetType.NPC);
        Assert.AreEqual(-1, characterMover.Health);
    }
}
```

## Testing
- Unity --> Window --> General --> TestRunner
- Test Runner window -> You should see your test file here (e.g. `TrapTests`)
- Click on desired test script -> Run selected
