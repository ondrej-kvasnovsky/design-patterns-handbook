# Factory

Factory pattern becomes handy we want to create instance based on some configuration. 

Lets say we got a file of various data, it is full of cats and dogs. We want to read the file and create specific objects based on a discriminator. Something like `AnimalFactory.create("DOG", "Alex")`.

```
abstract class Animal {
    public abstract String getName();
}

class Dog extends Animal {

    private String name;

    public Dog(String name) {
        this.name = name;
    }

    @Override
    public String getName() {
        return name;
    }
}

class Cat extends Animal {

    private String name;

    public Cat(String name) {
        this.name = name;
    }

    @Override
    public String getName() {
        return name;
    }
}

class AnimalFactory {

    public static Animal create(String type, String name) {
        if (type.equals("CAT")) {
            return new Cat(name);
        } else if (type.equals("DOG")) {
            return new Dog(name);
        }
        return null;
    }
}
```

Then we can easily create object using a static method `create`.

```
Animal cat = AnimalFactory.create("CAT", "Meow");
Animal dog = AnimalFactory.create("DOG", "Johnson");
```



