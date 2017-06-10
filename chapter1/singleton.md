# Singleton

Singleton provides a way to make sure there is only one instance of a class in one JVM instance.

Because many people used Singleton "everywhere", it became an anti-pattern. Singleton became an anti-pattern because it is difficult to create tests for code that has hundreds of singletons that make mocking really difficult. Another point against singletons are frameworks like Spring can make sure a bean is created only once and we do not need to write any extra code to achieve that.

> Before Java had enums, we used to implement singletons to mimic enums.

### Thread-safe singleton

Here is an example of thread-safe Singleton. Lets explain key points of this implementation:

* volatile is used to keep singleton in main memory, if volatile is not used, it could be in CPU cache, which could cause issues in multi-threaded environment
* constructor is private, only the singleton can instantiate it's self
* call of the constructor is synchronized on class level, to avoid two threads doing the same thing

```
public class ThreadSafeSingleton {

    private static volatile ThreadSafeSingleton instance;

    private ThreadSafeSingleton() {
    }

    public static ThreadSafeSingleton getInstance() {
        if (instance == null) {
            synchronized (ThreadSafeSingleton.class) {
                instance = new ThreadSafeSingleton();
            }
        }
        return instance;
    }
}
```

Here is how we get the instance of such a singleton.

```
ThreadSafeSingleton single = ThreadSafeSingleton.getInstance();
```

### Hack singleton

Anyway, there is a way to hack singleton and create new instances. We can use reflection and set the constructor as accesible.

```
ThreadSafeSingleton instanceOne = ThreadSafeSingleton.getInstance();
ThreadSafeSingleton instanceTwo = null;
try {
    Constructor[] constructors = ThreadSafeSingleton.class.getDeclaredConstructors();
    for (Constructor constructor : constructors) {
        //Below code will destroy the singleton pattern
        constructor.setAccessible(true);
        instanceTwo = (ThreadSafeSingleton) constructor.newInstance();
        break;
    }
} catch (Exception e) {
    e.printStackTrace();
}

// see, we got two instances that have different has codes!
System.out.println(instanceOne.hashCode());
System.out.println(instanceTwo.hashCode());
```



