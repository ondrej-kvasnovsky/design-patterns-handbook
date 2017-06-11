# Iterator

Iterator patterns provide way to access collection elements in sequential way.

### Example - Iterating through collection

We are going to implement our own iterator to to iterate. This way we will practice iterator pattern and see how an iterator can be created. 

```
interface Iterator<T> {
    boolean hasNext();
    T getNext();
}

class IteratorImpl<T> implements Iterator {

    private java.util.Iterator<T> values;

    public IteratorImpl(List<T> values) {
        this.values = values.iterator();
    }

    public boolean hasNext() {
        return values.hasNext();
    }

    public T getNext() {
        return values.next();
    }
}

// we could add Iterable interface and would define iterator() method
class UserCollection {

    private List<User> users = new ArrayList<>();

    public void add(User user) {
        users.add(user);
    }

    public Iterator<User> iterator() {
        return new IteratorImpl<>(users);
    }
}

class User {
    private String name;

    public User(String name) {
        this.name = name;
    }

    public String toString() {
        return name;
    }
}
```

Here is how we could use this iterator. 

```
UserCollection users = new UserCollection();
users.add(new User("John"));
users.add(new User("Jimmy"));

Iterator<User> iterator = users.iterator();
while (iterator.hasNext()) {
     User user = iterator.getNext();
     System.out.println(user);
}
```







