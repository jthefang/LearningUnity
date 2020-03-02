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