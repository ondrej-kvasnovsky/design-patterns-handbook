# Builder

The main reason to use Builder pattern is to avoid constructors with huge amount of parameters. Some people say we should have a limit for number of parameters and it should be around 3 parameters per constructor. We should keep the number parameters around 3 because if not, it is becoming a piece of code that is difficult to read, test and manage. People, who developed feelings for a code, might say that constructors with many parameters are ugly and not friendly.

The goal when using Builder patter is to create fluent interface. Here is an example of fluent interface:

```
new UserBuilder().withUserName("john").withAge(21).build();
```

### Example - User builder

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

### Builder in JavaScript using async / await

When we are writing integration tests, we might need to insert some data, that are expected to be present, into database. We can write a SQL script or put SQLs into code that would executed before each test. But then it becomes difficult to update this script. As we add more features we might need to have multiple SQL scripts to cover various scenarios. Better is to split SQL inserts into methods of builder and let the user, the one who writes the integration tests, to choose what should be inserted.

Since we want to threat the test code as production code, we should follow DRY principle. Then it becomes handy to put data creation into builders that we can reuse in test code. Here is a builder that will help us to insert data into database so we can run our integration tests.

```
module.exports = class {
  constructor() {
    this.promises = []
  }

  withUser() {
    this.promises.push(async () => {
      const userService = await Q.container.getAsync('userService')
      await userService.save(new User('John'))
    })
    return this
  }

  withAccount() {
    this.promises.push(async () => {
      const accountService = await Q.container.getAsync('accountService')
      await accountService.save(new Account('My Account'))
    })
    return this
  }

  async build() {
    for (let promise of this.promises) {
      await promise()
    }
  }
}
```

Then we can use the builder in a test as follows.

```
const TestDataBuilder = require('../../fixtures/test_data_builder')

describe('User Is Able to Access his Account', function() {

  beforeEach(async function() {
    await new TestDataBuilder().withUser().withAccount().build()
  })

  describe('login()', function() {
    it('fails to login because user is not linked with account', async function() {
      // write test code here
    })
  })
})
```



