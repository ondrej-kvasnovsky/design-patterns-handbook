# Define Method

Instead of having multiple lines of code with unclear purpose, we extract that code into to well named methods. By extracting these lines of code into methods, we usually give couple of variables clear scope and meaning.

Rules:

* a new methods needs to have a name that is easy to understand
* a new method can't use shortcuts or any abbreviations
* number of lines in method should be small, but there are some unique exceptions

> Any fool can write code that a computer can understand. Good programmers write code that humans can understand.  
> -- Martin Fowler

This pattern is not suggesting we can't have long methods. There are good reasons to have a long method that does one thing well, but maybe it is the nature of that one thing to be long. Here is an example that could be reasonable long method. In this example, lets say we are reading data from a datasource and would rather keep it atomic. I have seen these kinds of methods that were reading data from old mainframes in foreign language and the most readable way to keep them, was to keep that code together.

```
class DbEntry {
  int id;
  int index1;
  int index2;
  int index3;
  int index4;
  int index5;
  int index6;
  int index7;
  int index8;
  int index9;
  int index10;
}
public DbEntry build(CrazyOldEntity entity) {
  DbEntry entry = new DbEntry();
  entry.id = entity.getKey();
  entry.index1 = entity.getIKON();
  entry.index2 = entity.getKrekos();
  entry.index3 = entity.getIKA();
  entry.index4 = entity.getKiitos();
  entry.index5 = entity.getKiitoksia();
  entry.index6 = entity.getPerkele();
  entry.index7 = entity.getUksi();
  entry.index8 = entity.getOlut();
  entry.index9 = entity.getNimi();
  entry.index10 = entity.getMarenhaimen();
  return entry;
}
```

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



