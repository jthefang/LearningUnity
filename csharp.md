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