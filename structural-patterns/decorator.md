# Decorator

* Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.
* Client-specified embellishment of a core object by recursively wrapping it.
* Wrapping a gift, putting it in a box, and wrapping the box.

### Example - Cars

We create few classes and then we can dynamically build cars, without changing the code.

```
interface Car {
    void assemble();
}

class BasicCar implements Car {

    @Override
    public void assemble() {
        System.out.println("Basic car.");
    }
}

class CarDecorator implements Car {

    private Car car;

    public CarDecorator(Car car) {
        this.car = car;
    }

    @Override
    public void assemble() {
        car.assemble();
    }
}

class SportCar extends CarDecorator {

    public SportCar(Car car) {
        super(car);
    }

    @Override
    public void assemble() {
        super.assemble();
        System.out.println("Sport car.");
    }
}

class LuxuryCar extends CarDecorator {

    public LuxuryCar(Car car) {
        super(car);
    }

    @Override
    public void assemble() {
        super.assemble();

        System.out.println("Luxury car.");
    }
}
```

Here is how to build Mustang and LaFerrari using the classes wrapping. 

```
Car mustang = new SportCar(new BasicCar());
mustang.assemble();

System.out.println();

Car laFerrari = new SportCar(new LuxuryCar(new BasicCar()));
laFerrari.assemble();
```

The code prints out the following and we can observe how car was assembled. 

```
Basic car.
Sport car.

Basic car.
Luxury car.
Sport car.
```



