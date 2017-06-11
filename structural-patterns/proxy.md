# Proxy

* Provide a surrogate or placeholder for another object to control access to it.
* Use an extra level of indirection to support distributed, controlled, or intelligent access.
* Add a wrapper and delegation to protect the real component from undue complexity.

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



