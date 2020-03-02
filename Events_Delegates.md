# Delegates
A way to store functions in variables and pass them around in C#. Delegates declare function signature without defining the implementation.

```c#
public class Testing : MonoBehaviour {
    public delegate void TestDelegate(); //return type is void, no parameters to the delegate function
    public delegate vool TestBoolDelegate(int i);

    private TestDelegate testDelegateFunction; //can be assigned to any function that matches the signature of TestDelegate (i.e. void return and no parameters)
    private TestBoolDelegate testBoolDelegateFunction;

    /*
        Last type is always the return type (e.g. bool for these examples below)
    */
    private Func<bool> testFunc;
    private Func<int, bool>  testIntBoolFunc; //int parameter, bool return type

    void Start() {
        /* 
        Also:
        testDelegateFunction = new TestDelegate(MyTestDelegateFunction);

        testDelegateFunction = delegate() { Debug.Log("Anonymous method"); };

        Lambda expression:
        testDelegateFucntion = () => { Debug.Log("Anonymous method"); };
        */
        testDelegateFunction = MyTestDelegateFunction;
        /*
        * delegates can be multi cast (i.e. calling the delegate triggers multiple functions)
        * -= to remove functions
        * can't do this with lambda expressions
        */
        testDelegateFunction += MySecondDelegateFunction; 
        
        testDelegateFunction(); //logs "Hello" then logs "World"

        testBoolDelegateFunction = MyTestBoolDelegateFunction;
        /* 
        Lambda expression:
        testBoolDelegateFunction = (int i) => i < 5;
        testBoolDelegateFunction = (int i) => {
            ...some more code up here
            return i < 5; 
        };
        */
        Debug.Log(testBoolDelegateFunction(1));

        testFunc = () => false;
        testIntBoolFunc = (int i) => i < 5;
    }

    private void MyTestDelegateFunction() {
        Debug.Log("Hello");
    }

    private void MySecondDelegateFunction() {
        Debug.Log("World");
    }

    private bool MyTestBoolDelegateFunction(int i) {
        return i < 5;
    }

}
```

## Example: Timer countdown trigger
```c#
public class ActionOnTimer : MonoBehaviour {
    private Action timerCallback;
    private float timer;

    public void SetTimer(float timer, Action timerCallback) {
        this.timer = timer;
        this.timerCallback = timerCallback;
    }

    void Update() {
        if (timer > 0f) {
            timer -= Time.deltaTime;

            if (IsTimerComplete()) {
                timerCallback;
            }
        }
    }

    public bool IsTimerComplete() {
        return timer <= 0f;
    }
}

public class Testing : MonoBehaviour {
    [SerializeField]
    ActionOnTimer actionOnTimer;

    void Start() {
        acitonOnTimer.SetTimer(1f, () => { Debug.Log("Timer complete!"); });
    }
}
```

# What are events?
Events help decouple game logic from visual components. An event is published, and subscribers are notified.

# Code
```c#
public class SampleClass : MonoBehaviour {
    public event EventHandler OnSpacePressed;

    void Start() {
        OnSpacePressed += SomeSubscriberMethod; //subscribes to the event
        // do -= to unsubscribe from an event
    }

    void SomeSubscriberMethod(object sender, EventArgs e) { //Subscriber
        //do something on trigger
    }

    void SomeMethod() { // Publisher
        //a trigger happens
        if (Input.GetKeyDown(KeyCode.Space)) {
            /*if (OnSpacePressed != null) {
            //need this to check for subscribers (if no subscribers, null exception)
                OnSpacePressed(this, EventArgs.Empty);
            } // equivalent to below */
            OnSpacePressed?.Invoke(this, EventArgs.Empty); 
        }
    }
}

// For args
public class SampleClass : MonoBehaviour {
    public event EventHandler<OnSpacePressedEventArgs> OnSpacePressed;

    public class OnSpacePressedEventArgs : EventArgs {
        public int spaceCount;
    }

    private int spaceCount;

    void Start() {
        //...
    }

    void SomeSubscriberMethod(object sender, SampleClass.OnSpacePressedEventArgs e) { //Subscriber
        Debug.Log(e.spaceCount);
    }

    void SomeMethod() { // Publisher
        //a trigger happens
        if (Input.GetKeyDown(KeyCode.Space)) {
            spaceCount++;
            OnSpacePressed?.Invoke(this, new OnSpacePressedEventArgs{ spaceCount = spaceCount }); 
        }
    }
}
```

# Events and Delegates
Events work with delegates (just defines the event function signature). `EventHandler` is just a type of delegate. The default delegate is `Action`. You can also define your own delegate. 

```c#
using System; //for Action
public class SampleClass : MonoBehaviour {
    public event Action<bool, int> OnActionEvent; //you can define custom signature for subscriber
    //Action by itself is void return type and no params

    //own delegate
    public event TestEventDelegate OnFloatEvent;
    public delegate void TestEventDelegate(float f);

    void Start() {
        this.OnActionEvent += OnActionEvent_Subscriber;
        this.OnFloatEvent += OnFloatEvent_Subscriber;
    }

    void OnActionEvent_Subscriber(bool b, int i) {
        //do something
    }

    void OnFloatEvent_Subscriber(float f) { //Subscriber
        //do something 
    }

    void SomeMethod() { // Publisher
        //a trigger happens
        if (Input.GetKeyDown(KeyCode.Space)) {
            OnFloatEvent?.Invoke(5.5f);
            OnActionEvent?.Invoke(true, 56);
        }
    }
}
```

# UnityEvent
Just an event type that shows up in the Unity editor.

```c#
public class SampleClass : MonoBehaviour {
    public UnityEvent OnUnityEvent; 
    /**
        On this object --> Inspector --> Script Component --> + subscriber object 
        --> select the function to trigger on the subscriber (must have void signature in this case)
    */

    void Start() {
        this.OnActionEvent += OnActionEvent_Subscriber;
        this.OnFloatEvent += OnFloatEvent_Subscriber;
    }

    void OnActionEvent_Subscriber(bool b, int i) {
        //do something
    }

    void OnFloatEvent_Subscriber(float f) { //Subscriber
        //do something 
    }

    void SomeMethod() { // Publisher
        //a trigger happens
        if (Input.GetKeyDown(KeyCode.Space)) {
            OnFloatEvent?.Invoke(5.5f);
            OnActionEvent?.Invoke(true, 56);
        }
    }
}
```
