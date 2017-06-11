# Factory Method

Factory Method is using methods to create objects. Factory Method should define abstract methods in an abstract class or interface and classes that implement those are responsible for providing an instance.

### Example - Multi-platform UI

We want to create a class design where we support two different architectures, Mac and Windows. The example is oversimplified but should provide basic idea.

```
interface Dialog {
    void show();
}

abstract class AbstractDialog implements Dialog {

    abstract TextField createTextField();
    abstract Button createButton();
}

class MacDialog extends AbstractDialog {

    @Override
    public void show() {
        createTextField();
        createButton();
    }

    @Override
    TextField createTextField() {
        return new MacTextField();
    }

    @Override
    Button createButton() {
        return new MacButton();
    }
}

class WindowsDialog extends AbstractDialog {

    @Override
    public void show() {
        createTextField();
        createButton();
    }

    @Override
    TextField createTextField() {
        return new WindowsTextField();
    }

    @Override
    Button createButton() {
        return new WindowsButton();
    }
}

interface TextField {
}

class MacTextField implements TextField {
}

class WindowsTextField implements TextField {
}

interface Button {
}

class MacButton implements Button {
}

class WindowsButton implements Button {
}
```

Then we just use specific implementations of class that defined abstract factory methods.

```
Dialog dialog = new WindowsDialog();
Dialog dialog = new MacDialog();
```

### Example - Factory method with Template pattern

Usually, factory method is combined with Template pattern. Factory methods are responsible for object creation and Template for behavior of those newly created objects. We would have to implement `show` method in `AbstractDialog` that will represent the template that uses the newly created objects. Then we should remove implementation of `show` method in `WindowsDialog` and `MacDialog`.

```
abstract class AbstractDialog implements Dialog {

    @Override
    public void show() {
        createTextField().position(0, 0).show();
        createButton().position(0, 100).show();
    }

    abstract TextField createTextField();
    abstract Button createButton();
}
```



