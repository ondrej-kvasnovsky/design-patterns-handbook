# Abstract Factory

The abstract factory pattern provides a way to encapsulate a group of individual factories that have a common theme without specifying their concrete classes.

```
interface Animal {
    String getName();
}

class Cat implements Animal {

    private String name;

    public Cat(String name) {
        this.name = name;
    }

    @Override
    public String getName() {
        return name;
    }
}

class Dog implements Animal {

    private String name;

    public Dog(String name) {
        this.name = name;
    }

    @Override
    public String getName() {
        return name;
    }
}

interface AnimalAbstractFactory {
    Animal create();
}

class CatFactory implements AnimalAbstractFactory {

    private String name;

    public CatFactory(String name) {
        this.name = name;
    }

    @Override
    public Animal create() {
        Cat cat = new Cat(name);
        return cat;
    }
}

class DogFactory implements AnimalAbstractFactory {

    private String name;

    public DogFactory(String name) {
        this.name = name;
    }

    @Override
    public Animal create() {
        return new Dog(name);
    }
}

class AnimalFactory {

    public static Animal create(AnimalAbstractFactory factory) {
        Animal animal = factory.create();
        return animal;
    }
}
```

> `AnimalAbstractFactory` can be implemented as interface or abstract class, it depends what is more useful for your problem.

Then we need to create an object, we create specific factory and as a user of such code, we no idea what instance is going to be created \(because probably we shouldn't care\).

```
Animal meow = AnimalFactory.create(new CatFactory("Meow"));
Animal gilbert = AnimalFactory.create(new DogFactory("Gilbert"));
```



