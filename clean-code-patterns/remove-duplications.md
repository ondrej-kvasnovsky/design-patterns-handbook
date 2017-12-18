# Remove Duplications

We need to only remove duplications that are somehow related. Code that is somehow related is a code that shares common domain.

Lets explain that on the following examples. 

### Reuse code which is already there

If we have a class that is doing the same thing but returning different type of results, we definitely want to remove duplicates. 

We can see here that the `executeAndGetString` method and `executeAndGetDouble` methods are used for the same purpose, they share the domain. They are both used for the same purpose, to get average of certain set of numbers.

```
public class Executor {

    public String executeAndGetString(int i) {
        OptionalDouble average = IntStream.range(0, i).average();
        return String.valueOf(average.getAsDouble());
    }

    public double executeAndGetDouble(int i) {
        OptionalDouble average = IntStream.range(0, i).average();
        return average.getAsDouble();
    }
}
```

We will refactor that code and remove duplications. Then we actually find we can reuse `extractAndGetDouble` method.

```
class RefactoredExecutor {

    public String executeAndGetString(int i) {
        return String.valueOf(executeAndGetDouble(i));
    }

    public double executeAndGetDouble(int i) {
        OptionalDouble average = IntStream.range(0, i).average();
        return average.getAsDouble();
    }
}
```

### Do not remove duplicates always

There are some occasions, when we shouldn't reuse the code and we should rather keep it separate. This can be difficult to judge, but lets decide after we see this example. 

In this example, there are two different entities, Apple and Car. These two different entities, have different methods `getName` and `getCount`. Anyway, it seems there is a code duplication. What happens if we remove this duplicated code?

```
import java.util.stream.LongStream;

class Apple {
    private long size;

    public String getName() {
        long count = LongStream.range(0, size).count();
        return String.valueOf(count);
    }
}

class Car {
    private long power;

    public String getCount() {
        long count = LongStream.range(0, power).count();
        return String.valueOf(count);
    }
}
```

First option to remove duplicate is to create a parent for both classes. 

```
class Parent {
    public String getString(long number) {
        long count = LongStream.range(0, number).count();
        return String.valueOf(count);
    }
}

class Apple extends Parent {
    private long size;

    public String getName() {
        return getString(size);
    }
}

class Car extends Parent {
    private long power;

    public String getCount() {
        return getString(power);
    }
}
```

That would be probably the worst thing we could do. We would make very strong relationship between tho classes that had nothing in common previously. Since this is not the way, what else we can do?

We can replace inheritance by composition. That means, we can move the code to common class, like `Utils`, and call it when ever we need to get the string. The following solution is better than the one using inheritance. But still, it says there is some kind of relationship between `Apple` and `Car`. But in the reality, there is no relation ship between `Apple` and `Car`. 

```
class Utils {
    public static String getString(long number) {
        long count = LongStream.range(0, number).count();
        return String.valueOf(count);
    }
}

class Apple {

    private long size;

    public String getName() {
        return Utils.getString(size);
    }
}

class Car {

    private long power;

    public String getCount() {
        return Utils.getString(power);
    }
}
```

Other point to consider is: what would happen if I have to change behavior of `getName` method in `Apple` class. I would have to update `Utils.getString` method, but then I would change behavior of Car class. 

One solution could be add another method to the `Utils` method and add the new requested behavior there. This is would be most tempting and usually is most preferred, because it follows the same principle, keep these kind of methods in `Utils` class. Many junior developers would follow this way. But it is wrong. Firtst, it creates duplicated code and second, it is moving code that belongs to `Car` and `Apple` classes somewhere else. It makes the code less understadable. Also, what if someone decides to reuse this code in `Universe` class? We are not far away from a disaster.

```
class Utils {
    public static String getStringForCar(long number) {
        long count = LongStream.range(0, number).count();
        return String.valueOf(count);
    }
    public static String getStringForApple(long number, String otherInput) {
        long count = LongStream.range(0, number).count();
        return otherInput + " " + String.valueOf(count);
    }
}
```

What is the other solution? The logical implication is to stop using `Utils.getName` in `Apple` class and create its own implementation. But then we would end up with the same code as we started, with duplicates. And we would probably make it more complicated for other developers. They could ask, "Why did they created Utils method if the method is used only on one place? That makes no sense!".

Reusing code and removing duplicated is very important, but everything has its price. It is up to us to judge and decide, how much are we willing to pay.

