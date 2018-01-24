# Composition over Inheritance

Better to reference other classes using composition than binding code together using inheritance. 

### The problem

Lets look at crazy example that would over use inheritance. 

```
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

@RestController
@RequestMapping("/")
public class Controller extends Service {

    @GetMapping("users")
    public ResponseEntity<List<User>> users() {
        return ResponseEntity.ok(getUsers());
    }
}

class Service extends Repository {
    List<User> getUsers() {
        List<String> users = super.findUsers();
        return users.stream().map(User::new).collect(Collectors.toList());
    }
}

class User {
    private final String name;

    User(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

class Repository {
    List<String> findUsers() {
        ArrayList<String> users = new ArrayList<>();
        users.add("John");
        return users;
    }
}

```

### Solution

The solution is to use composition. The code becomes more testable. 

> To make the code really testable, we would probably have to create couple interfaces for the service and repository. That would allow us to mock the code and write proper unit tests.

```
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

@RestController
@RequestMapping("/")
public class CompoController {
    private final Service service;

    public CompoController(Service service) {
        this.service = service;
    }

    @GetMapping("users")
    public ResponseEntity<List<User>> users() {
        List<User> users = service.getUsers();
        return ResponseEntity.ok(users);
    }
}

@org.springframework.stereotype.Service
class Service {
    private final Repository repository;

    Service(Repository repository) {
        this.repository = repository;
    }

    List<User> getUsers() {
        List<String> users = repository.findUsers();
        return users.stream().map(User::new).collect(Collectors.toList());
    }
}

class User {
    private final String name;

    User(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

@org.springframework.stereotype.Repository
class Repository {
    List<String> findUsers() {
        ArrayList<String> users = new ArrayList<>();
        users.add("John");
        return users;
    }
}
```



