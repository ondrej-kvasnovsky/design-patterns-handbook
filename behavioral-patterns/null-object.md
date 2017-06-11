# Null Object

The intent of a Null Object is to encapsulate the absence of an object by providing a substitutable alternative that offers suitable default do nothing behavior. In short, a design where "nothing will come of nothing"

### Example - Empty iterator instead of null

We are going to use null object to represent empty iterator. Then we use this iterator whenever we would want to return null.

```
interface Iterator<T> {
    boolean hasNext();
    T next();
}

class IteratorImpl<T> implements Iterator {

    @Override
    public boolean hasNext() {
        return true; // TODO: here would be checking if the collection has more items to iterate through
    }

    @Override
    public T next() {
        return null;
    }
}

class NullIterator<T> implements Iterator {

    @Override
    public boolean hasNext() {
        return false;
    }

    @Override
    public T next() {
        return null;
    }
}
```

Instead of returning or working with null, we return instance of `NullIterator`.

```
class MyCollection {
    private List list;

    public Iterator iterator() {
        if (list == null) {
            return new NullIterator();
        }
        return new IteratorImpl(); // we would have to finish implementation of this iterator
    }
}
```



