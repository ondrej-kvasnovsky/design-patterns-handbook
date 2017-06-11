# Private Class Data

* Control write access to class attributes
* Separate data from methods that use it
* Encapsulate class data initialization
* Providing new type of `final` - final after constructor

### Example - Hide attributes by keeping them elsewhere

In this example, we want to hide user's password variable. 

```
class User {
    private UserData userData;

    public User(String name, String password) {
        userData = new UserData(name, password);
    }

    public String getName() {
        return userData.getName();
    }
}

class UserData {
    private String name;
    private String password;

    public UserData(String name, String password) {
        this.name = name;
        this.password = password;
    }

    public String getName() {
        return name;
    }
}
```

User of the code does not have to know how is data internally stored in class, he uses the code as normally. But he can't access `getPassword()` method anywhere. 

```
User john = new User("John", "secret");
```

> This is simplified example, we could be hiding variables, methods or some functionality at initialization of an object.



