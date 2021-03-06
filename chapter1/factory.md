# Factory

Factory pattern becomes handy we want to dynamically create instance based on some inputs.

### Example - Animal factory

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

If there are too many variables that should be passed into create method, we might want to combine this pattern with Builder pattern. When we call `create` method, it would return `AnimalBuilder`. It would become something like this:`AnimalBuilder buidler = AnimalFactory.create("DOG");`

## Example - Factory in JavaScript

Here is an example of factory function that creates a specific instance of image class. 

```
function createImage(name) {
  if (name.match(/\.jpeg$/)) {
    return new JpegImage(name);
  } else  if (name.match(/\.png/)) {
    return new PngImage(name);
  } else {
    throw new Error('Unsupported image format')
  }
}

class JpegImage {
  constructor(name) {
    this.name = name;
  }
}

class PngImage {
  constructor(name) {
    this.name = name;
  }
}

const jpeg = createImage('my.jpeg');
console.log(jpeg instanceof JpegImage);

const png = createImage('my.png');
console.log(png instanceof PngImage);

createImage('my.gif');
```

## Example - Enforce encapsulation

This example shows factory method that not only creates a new instances, but also hides some properties from the user. 

    function createImage(name) {
      if (name.match(/\.jpeg$/)) {
        return createJpeg(name);
      } else  if (name.match(/\.png/)) {
        return createPng(name);
      } else {
        throw new Error('Unsupported image format')
      }
    }

    function createPng(name) {
      const privateProperties = {};

      class PngImage {
        constructor(name) {
          this.name = name;
          privateProperties.veryInternal = `very special and internal thing that nobody can change`
        }
        getSomethingSpecialThatNobodyCanModify() {
          return privateProperties.veryInternal;
        }
      }
      const image = new PngImage(name);
      return image;
    }

    function createJpeg(name) {
      const privateProperties = {};
      class JpegImage {
        constructor(name) {
          this.name = name;
          privateProperties.veryInternal = `another very special and internal thing that nobody can change`
        }
        getSomethingSpecialThatNobodyCanModify() {
          return privateProperties.veryInternal;
        }
      }

      const image = new JpegImage(name);

      return image;
    }

    const jpeg = createImage('my.jpeg');
    console.log(jpeg);
    console.log(jpeg.getSomethingSpecialThatNobodyCanModify());

    const png = createImage('my.png');
    console.log(png);
    console.log(png.getSomethingSpecialThatNobodyCanModify());



