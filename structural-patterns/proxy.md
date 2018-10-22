# Proxy

A proxy is an object that controls access to another object. They both must have the same interface \(methods and methods signatures\).

* Provide a surrogate or placeholder for another object to control access to it.
* Use an extra level of indirection to support distributed, controlled, or intelligent access.
* Add a wrapper and delegation to protect the real component from undue complexity.

A proxy is useful to do data validation, security, caching, logging, lazy initialization or implement a remote object logic.

### Example - Secured commands

We are going to use Proxy pattern to secure some commands that should be accessible only for administrators. `CommandExecutorProxy` class makes sure the user who is trying to execute a command is the administrator.

```
interface CommandExecutor {
    void execute(String s);
}

class CommandExecutorImpl implements CommandExecutor {

    @Override
    public void execute(String s) {
        System.out.println("> " + s);
    }
}

class CommandExecutorProxy implements CommandExecutor {

    private CommandExecutor executor = new CommandExecutorImpl();

    private boolean isAdmin;

    public CommandExecutorProxy(String username, String password) {
        // TODO: find out if user is admin
        isAdmin = true;
    }

    @Override
    public void execute(String command) {
        if (isAdmin) {
            executor.execute(command);
        } else {
            throw new RuntimeException("Not authorized!");
        }
    }
}
```

Here is how we could use the code created above. If `john` is not an admin, the application should throw exception.

```
CommandExecutor executorProxy = new CommandExecutorProxy("john", "my-secret");
executorProxy.execute("ls -l");
```

## Example - Proxy in JavaScript

Here is an example how to implement Proxy in JavaScript.

```
function createProxy(subject) {
  const proto = Object.getPrototypeOf(subject)
  function Proxy(subject) {
    this.subject = subject;
  }
  Proxy.prototype = Object.create(proto);
  // proxies function
  Proxy.prototype.hello = function() {
    return this.subject.hello() + ' there!';
  }
  // delegate function
  Proxy.prototype.goodbye = function() {
    return this.subject.goodbye.apply(this.subject, arguments)
  }
  return new Proxy(subject);
}

class MyObject {
  hello() {
    return "Hello";
  }
  goodbye() {
    return "Goodbye";
  }
}

const myObject = new MyObject();
const myProxiedObject = createProxy(myObject);
console.log(myProxiedObject instanceof MyObject); // return true
console.log(myProxiedObject.hello());
console.log(myProxiedObject.goodbye());
```

Or there is easier way to create proxy, taking advantage of dynamic typing in JavaScript.

```
function createProxy(subject) {
  return {
    hello: () => subject.hello() + ' there!',
    goodbye: () => subject.goodbye(),
  };
}

const myObject = new MyObject();
const myProxiedObject = createProxy(myObject);
console.log(myProxiedObject instanceof MyObject); // return false
console.log(myProxiedObject.hello());
console.log(myProxiedObject.goodbye());
```

ES2015 contains Proxy implementation. 

```
class MyObject {
  constructor(name) {
    this.name = name;
  }
}
const myObj = new MyObject("Celeste");
const myProxied = new Proxy(myObj, {
  get: (target, property) => target[property].toUpperCase()
});
console.log(myProxied.name); // prints "CELESTE"
```



