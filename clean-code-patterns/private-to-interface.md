# Private to Interface

The code grows as the code is maintained and enhanced. If there is not enough refactoring and deliberate actions, quite few private methods can pop up there. That would be probably fine but more private methods we have, more difficult the testing is. If we have 1 public method in one class and none private methods, it is very easy to test it because there is nothing hidden. But if we have a class with 1 public method and 10 private methods that do all the work, it can become very difficult to test all the if-else or try-catch branches. 

Consider the following scenario. We have a class that has about 1 000 lines and contains 1 public method and 10 private methods. We change the logic of one private method in order to fix a bug. Then we run the test and nothing fails. We found that that part of code is not covered by tests. We have to options. First, try to create the test and get into that branch, which might be very difficult and even if we do it, we will have to probably prepare a lot of dummy data or we will have to do a lot of mocking. In some cases, we might be forced to do both. If we do write a test that requires a lot of mocking, the readability and maintainability of that test is absolutely not good. We always need to keep in mind that there will be other people changing the code and also the tests should be treated as production code. The other option is to move that private method somewhere else and make it testtable. We can create an interface that contains that method definition. Then we create implementation of that interface and we move the private method there and thus we make it public. Then we can create unit test and test the new class. In the existing code, we reference the new interface. 

### Example

Lets consider we have the following code and we want to change `findRole` method.

```
class Db {
   public String execute(String sql) {
       return "dummy role";
   }
}

class User {
    private int id;
    private String role;

    public User(int id, String role) {
        this.id = id;
        this.role = role;
    }
}

class UserService {

    private Db db;

    public User createUser(int id, int roleId) {
        validate(id);
        String role = findRole(roleId);
        return new User(id, role);
    }

    private void validate(int id) {
        if (id <= 0) throw new RuntimeException("Not valid ID, use positive number");
    }

    private String findRole(int roleId) {
        return db.execute("SELECT role FROM roles WHERE role_id = " + roleId);
    }
}
```

Instead of updating tests for UserService we create a new set of classes to handle logic of`findRole`method.

```
class Db {
    public String execute(String sql) {
        return "dummy role";
    }
}

class User {
    private int id;
    private String role;

    public User(int id, String role) {
        this.id = id;
        this.role = role;
    }
}

interface RoleService {
    String findRole(int userId);
}

class DefaultRoleService implements RoleService {

    private Db db = new Db();

    @Override
    public String findRole(int roleId) {
        if (roleId <= 0) throw new RuntimeException("Role ID needs to be positive.");
        return db.execute("SELECT role FROM roles WHERE role_id = " + roleId);
    }
}

class NewUserService {

    private RoleService roleService = new DefaultRoleService();

    public User createUser(int id, int roleId) {
        validate(id);
        String role = roleService.findRole(roleId);
        return new User(id, role);
    }

    private void validate(int id) {
        if (id <= 0) throw new RuntimeException("Not valid ID, use positive number");
    }
}
```





