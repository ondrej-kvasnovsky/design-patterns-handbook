# Over Patternized

Over patternization happens when we go wild and we use every design pattern we have heard of without applying any reasoning.

### Example - Hello World

Lets say we want to print `Hello World!` message. One, simple approach would be this one. Just print out the message, which should be enough for the first version of the hello world application.

```
class PrintHelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

The other approach is to throw there couple of design patterns. We are going to use `Facade` pattern because amount of classes is too high to expect someone would try to understand what is happening under the hood. `Facade` patter helps us to wrap all the logic. It first creates a message using `Builder` pattern pattern. Then it acquires command factory using `Factory` and `Singleton` patterns. Then we create a new command that will be responsible for printing out the text, `Command` pattern. When we have command pattern, we just call execute which prints out the message. 

```
class PrintHelloWorld {
    public static void main(String[] args) {
        Facade facade = new Facade();
        facade.print("Hello World!");
    }
}

class Facade {
    public void print(String message) {
        Message m = new MessageBuilder().withMessage(message).build();
        CommandFactory commandFactory = SystemOutPrintlnCommandFactory.getInstance();
        Command command = commandFactory.createCommand(m);
        command.execute();
    }
}

interface Message {
    String getMessage();
    void setMessage(String message);
}

class DefaultMessage implements Message {
    private String message;

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}

class MessageBuilder {
    private final Message message;

    public MessageBuilder() {
        this.message = new DefaultMessage();
    }

    public MessageBuilder withMessage(String message) {
        this.message.setMessage(message);
        return this;
    }

    public Message build() {
        return message;
    }
}

interface CommandFactory {
    Command createCommand(Message message);
}

class SystemOutPrintlnCommandFactory implements CommandFactory {

    private static SystemOutPrintlnCommandFactory instance = new SystemOutPrintlnCommandFactory();

    public static CommandFactory getInstance() {
        return instance;
    }

    private SystemOutPrintlnCommandFactory() {}

    @Override
    public Command createCommand(Message message) {
        if (message instanceof DefaultMessage) {
            return new SystemOutPrintlnCommand(message);
        }
        throw new RuntimeException("No suitable command found");
    }
}

interface Command {
    void execute();
}

class SystemOutPrintlnCommand implements Command {

    private Message message;

    public SystemOutPrintlnCommand(Message message) {
        this.message = message;
    }

    @Override
    public void execute() {
        System.out.println(message.getMessage());
    }
}
```



