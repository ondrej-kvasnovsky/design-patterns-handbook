# Command

* Encapsulate a request as an object, thereby letting you parametrize clients with different requests, queue or log requests, and support undoable operations.
* Promote "invocation of a method on an object" to full object status
* An object-oriented callback

### Example - Unify REST and RCP calls

Command patter is going to help us to encapsulate REST and RCP call. 

```
class State {

    private String data1;
    private String data2;

    public String getData1() {
        return data1;
    }

    public void setData1(String data1) {
        this.data1 = data1;
    }

    public String getData2() {
        return data2;
    }

    public void setData2(String data2) {
        this.data2 = data2;
    }
}

interface Command {
    void execute();
}

class RestCommand implements Command {

    private State state;

    public RestCommand(State state) {
        this.state = state;
    }

    @Override
    public void execute() {
        System.out.println(getClass().getSimpleName() + " executing...");
    }
}

class RPCCommand implements Command {

    private State state;

    public RPCCommand(State state) {
        this.state = state;
    }

    @Override
    public void execute() {
        System.out.println(getClass().getSimpleName() + " executing...");
    }
}

class CommandExecutor {

    private List<Command> commands = new ArrayList<>();

    public void add(Command command) {
        commands.add(command);
    }

    public void execute() {
        commands.stream().forEach(Command::execute);
    }
}
```

Here is how we could call the commands. But we could also run just one command, it does not mean we have to always put all command into a collection of commands. 

```
State state = new State();

Command restCommand = new RestCommand(state);
Command rpcCommand = new RPCCommand(state);

CommandExecutor executor = new CommandExecutor();
executor.add(restCommand);
executor.add(rpcCommand);
executor.execute();
```

Here is the output. 

```
RestCommand executing...
RPCCommand executing...
```



