# Coroutines
- A function that is executed in intervals 
    - yield statements return execution out of the function, then when function continues, it picks up from exactly where it left off 
```c#
void Start() {
    StartCoroutine(MyCoroutine(target));
}

IEnumerator MyCoroutine(Transform target) {
    while (Vector3.Distance(transform.position, target.position) > 0.05f) {
        transform.position = Vector3.Lerp(transform.position, target.position, smoothing * Time.deltaTime);
        yield return null; //yield execution, code will resume at next (fixed)update (since null)
    }

    print("Reached the target");
    yield return new WaitForSeconds(3f); //yield then wait for 3 seconds before resuming 
    print("MyCoroutine is now finished");
}
```

# Getters and Setters
```c#
public string Username { get; set; } //normal way of get/setters

private string _password; //custom way
[Required]
public string Password
{
    get
    {
        return this._password;
    }

    set
    {  
        _password = Infrastructure.Encryption.SHA256(value);                
    }
}
```

# String formatting

## Numbers
```c#
float num = 1000000.35;
Debug.Log(num.ToString("n2")); //prints 1,000,000.35
Debug.Log(num.ToString("n0")); //prints 1,000,000
```