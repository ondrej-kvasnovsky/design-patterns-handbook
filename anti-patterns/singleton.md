# Singleton

Singleton pattern is mentioned in both good patterns and also in anti patterns. Why? I think because singleton is easy to implement and many people wanted to use design patterns where ever it is possible, even when they didn't have a need to solve an issue. Then we ended up with code that is:

* difficult to use
* complex
* difficult to test

Some people suggest that Singleton is an anti-pattern because it brings more evil than good. Still, Singleton pattern is useful when it is used correctly. 

A lesson to learn here is "Use Singleton only when you face an issue, never just because you think it might make the code more cool or fancy". 

### Example of Singleton pattern

How to create an anti pattern of Singleton pattern? It should be easy. We just need to use Singleton pattern when we shouldn't use it. 

```
public class UserFactory {

    private static UserFactory userFactory;

    private UserFactory() {
    }

    public static UserFactory getInstance() {
        if (userFactory == null) userFactory = new UserFactory();
        return userFactory;
    }

    public User createUser(String name) {
        return new User(name);
    }
}

class User {
    String name;

    public User(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

class Main {
    public static void main(String [] args) {
        User john = UserFactory.getInstance().createUser("John");
        System.out.println(john.getName());
    }
}
```

There is no reason why a developer who is using this this code couldn't create an instance of `User` class by him self. The code above seems to be over-engineered.

