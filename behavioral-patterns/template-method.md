# Template Method

Template method defines the skeleton of an algorithm in an operation, deferring some steps to client subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

Base class declares algorithm 'placeholders', and derived classes implement the placeholders.

### Example - Building a house

We are going to make "algorithm" to build a house. First we want to build foundation, then walls, then windows and roof as the last thing.

```
abstract class HouseTemplate {

    public void build() {
        buildFoundation();
        buildWalls();
        buildWindows();
        buildRoof();
    }

    protected abstract void buildFoundation();
    protected abstract void buildWalls();
    protected abstract void buildWindows();
    protected abstract void buildRoof();
}

class WoodenHouse extends HouseTemplate {

    @Override
    protected void buildFoundation() {
        System.out.println("Foundation");
    }

    @Override
    protected void buildWalls() {
        System.out.println("Wooden Walls");
    }

    @Override
    protected void buildWindows() {
        System.out.println("Windows");
    }

    @Override
    protected void buildRoof() {
        System.out.println("Wooden Roof");
    }
}
```

Then we build a wooden house like this. 

```
WoodenHouse house = new WoodenHouse();
house.build();
```

### Default implementations

We might find that some steps are the same in all implementations of template class. That might be case to put these implementations up, to the template class. We might move `buildFoundation` and `buildRoof` into template class because those could be implemented in the same way for all house types. And if not, then we can always override `buildFoundation` and `buildRoof` methods.

```
abstract class HouseTemplate {

    public void build() {
        buildFoundation();
        buildWalls();
        buildWindows();
        buildRoof();
    }

    protected abstract void buildFoundation() {
        System.out.println("Foundation");
    }
    
    protected abstract void buildWalls();
    
    protected void buildWindows() {
        System.out.println("Windows");
    }

    protected abstract void buildRoof();
}

class WoodenHouse extends HouseTemplate {

    @Override
    protected void buildWalls() {
        System.out.println("Wooden Walls");
    }

    @Override
    protected void buildRoof() {
        System.out.println("Wooden Roof");
    }
}
```





