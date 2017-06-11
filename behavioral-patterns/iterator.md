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

### Example - Accessing database rows

Here we create another iterator to move away from "iterator" keyword. It this case, ResultSet is the iterator. 

```
import java.util.*;

// simplified result set that returns on next string
interface ResultSet {
    String nextString();
    boolean hasNext();
}

class OneRowResultSet implements ResultSet {

    private Queue<String> stack;

    public OneRowResultSet(List<String> values) {
        this.stack = new ArrayDeque<>();
        stack.addAll(values);
    }

    @Override
    public String nextString() {
        return stack.poll();
    }

    @Override
    public boolean hasNext() {
        return !stack.isEmpty();
    }
}
```

Now we can use result set, fill it with values and iterate through it. 

```
List<String> values = new ArrayList<>();
values.add("A");
values.add("B");
values.add("C");

ResultSet resultSet = new OneRowResultSet(values);

while (resultSet.hasNext()) {
     String value = resultSet.nextString();
     System.out.println(value);
}
```

The code above goes through all records and prints out the following. 

```
A
B
C
```



