# Define Method

Instead of having multiple lines of code with unclear purpose, we extract that code into to well named methods. By extracting these lines of code into methods, we usually give couple of variables clear scope and meaning. 

## Example

Lets say we have the following code.

```
public String getNameWithBirthDate() {
    java.util.Calendar c = java.util.Calendar.getInstance();
    c.set(2005, java.util.Calendar.NOVEMBER, 20);

    String name = "John ";
    name += c.toInstant().toString();
    return name;
}
```

The code gets messy when we start playing with `Calendar` class. When we apply `Define Method` pattern, we extract all the code to separate unit of functionality.

```
public String getNameWithBirthDate() {
  Instant instant = getBirthDate();
  return getName(instant);
}

private String getName(Instant instant) {
  String name = "John ";
  name += instant.toString();
  return name;
}

private Instant getBirthDate() {
  java.util.Calendar c = java.util.Calendar.getInstance();
  c.set(2005, java.util.Calendar.NOVEMBER, 20);
  return c.toInstant();
}
```



