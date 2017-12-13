# Define Method

Instead of having multiple lines of code, we extract it to well named method. 

Example

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

```



