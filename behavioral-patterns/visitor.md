# Visitor

Represent an operation to be performed on the elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.

### Example - Shopping cart

We will implement a shopping cart full of vegetables that we will be able to visit and get its weight \(so we can then get the price for it\). 

```
interface Visitorable {
    int accept(Visitor visitor);
}

interface Visitor {
    int getWeight(Apple apple);
    int getWeight(Banana banana);
}

class VisitorImpl implements Visitor {

    @Override
    public int getWeight(Apple apple) {
        return apple.getWeight();
    }

    @Override
    public int getWeight(Banana banana) {
        return banana.getWeight();
    }
}

class Apple implements Visitorable {

    private int weight;

    public Apple(int weight) {
        this.weight = weight;
    }

    public int getWeight() {
        return weight;
    }

    @Override
    public int accept(Visitor visitor) {
        return visitor.getWeight(this);
    }
}

class Banana implements Visitorable {

    private int weight;

    public Banana(int weight) {
        this.weight = weight;
    }

    public int getWeight() {
        return weight;
    }

    @Override
    public int accept(Visitor visitor) {
        return visitor.getWeight(this);
    }
}
```

Now we are going to create apple and banana and visit them in order to get its weight. It is going to return sum equal to 250.

```
Visitor visitor = new VisitorImpl();
Visitorable[] visitorables = {new Apple(100), new Banana(150)};
Long sum = Arrays
                .stream(visitorables)
                .collect(Collectors.summarizingInt(it -> it.accept(visitor)))
                .getSum();
System.out.println(sum);
```



