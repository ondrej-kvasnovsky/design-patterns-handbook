# Builder

The main reason to use Builder pattern is to avoid constructors with huge amount of parameters. Some people say we should have a limit for number of parameters and it should be around 3 parameters per constructor. We should keep the number parameters  around 3 because if not, it is becoming a piece of code that is difficult to read and manage. People who developed feelings for a code, might say that constructors with many parameters are ugly and not friendly.

The goal when using Builder patter is to create fluent interface. Here is an example of fluent interface:

```
new UserBuilder().withUserName("john").withAge(21).build();
```

Here is an example a build that constructs a user. 

```
public class User {

    private String firstName;
    private String lastName;

    private User(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public static class UserBuilder {

        private String firstName;
        private String lastName;

        public UserBuilder withFirstName(String firstName) {
            this.firstName = firstName;
            return this;
        }

        public UserBuilder withLastName(String lastName) {
            this.lastName = lastName;
            return this;
        }

        public User build() {
            return new User(firstName, lastName);
        }
    }
}
```

Then we can use the builder's fluent interface to create a new object of `User` class.

```
User john = new User.UserBuilder()
    .withFirstName("John")
    .withLastName("Feleti")
    .build();
```



