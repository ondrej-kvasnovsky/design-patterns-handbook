# Flyweight

Flyweight is inteded to use sharing to support large numbers of fine-grained objects efficiently.

Good example to explain usage Flyweight is UIKit by Apple. When we create TableView that is can display milions rows to the user, we know that user can see only few at the time. What Apple engineers did that they are reusing components that are on the table view to avoid any performance issues.

### Example 

When our UI table should display a button, it will use ButtonFactory to get a button \(keeping track of what buttons are not used and visible is omitted\). Number that is the parameter in `get` method could be an ID of a button in a table row \(but it is not really important in this example\).

This example intent is to show that Flyweight is some kind of cash for objects that are memory-heavy or difficult to create.

```
class Button {
}

class ButtonFactory {

    private Map<Integer, Button> cache = new HashMap<>();

    public Button get(Integer number) {
        if (cache.containsKey(number)) {
            return cache.get(number);
        } else {
            Button button = new Button();
            cache.put(number, button);
            return button;
        }
    }
}
```

Then we would use the factory whenever we need an instance of a button that can be reused. 

```
ButtonFactory buttonFactory = new ButtonFactory();

Button button1a = buttonFactory.get(1);
Button button1b = buttonFactory.get(1);
```



