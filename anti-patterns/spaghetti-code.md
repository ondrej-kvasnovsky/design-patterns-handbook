# Spaghetti Code {#firstHeading}

We have spaghetti code in the project when the flow is conceptually like a bowl of spaghetti, i.e. twisted and tangled. There are other [Italian type of code](https://en.wikipedia.org/wiki/Spaghetti_code) as well.

### Example

With spaghetti code, we never know where the code starts and where the code ends. 

```
class DefaultModule {
    public static Processor proc() {
        return new Processor();
    }
}

class IQModule {
    public Processor process() {
        Processor proc = DefaultModule.proc();
        return proc;
    }
}

class Processor {
    public void exec() {
        Processor process = new IQModule().process();
        process.exec();
    }
}
```



