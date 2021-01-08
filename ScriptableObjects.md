Good intro video: https://www.youtube.com/watch?v=lJxy3oTZeCs

# What are they
- They are a shared data asset
  - If you want all encapsulated data to be shared across different kinds of objects
- Useful for storing constant/shared data (e.g. total health, speed) that is used frequently

# How to use
- Create the ScriptableObject:
```c#
[CreateAssetMenu(menuName="Vehicle Setting")]
public class VehicleSettings : ScriptableObject {
    public float speed;
    public float acceleration;
}
```
- Assets > Create > `Vehicle Setting`
    - Give values to `speed` and `acceleration` in the Inspector
- Read these values in your MonoBehaviour
    - the `SerializeField` `VehicleSettings` creates an object property on the component => can drag `Vehicle Setting` asset into this field in the Inspector
```c#
public class CarController : MonoBehaviour {
    [SerializeField]
    VehicleSettings settings;
    
    void Update() {
        _agent.speed = settings.speed;
        _agent.acceleration = settings.acceleration;
    }
}
```
- You can adjust the ScriptableObject asset values in play mode and the changes will persist even after exiting play mode (thank god)