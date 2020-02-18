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

# Delegates
Events work with delegates (just defines the event function signature). `EventHandler` is just a type of delegate. The default delegate is `Action`. You can also define your own delegate. 

```c#
using System; //for Action
public class SampleClass : MonoBehaviour {
    public event Action<bool, int> OnActionEvent; //you can define custom signature for subscriber

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
