# Observer

Observer defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

### Example - Propagate status change

When user status changes, we need to do many things: update UI, publish message so other users are notified about status change and maybe we know that we will add more reactions later on. 

```
class StatusSubject {

    private int status;

    private List<StatusObserver> observers = new ArrayList<>();

    public void add(StatusObserver observer) {
        observers.add(observer);
    }

    public void updateStatus(int status) {
        this.status = status;
    }

    public void notifyObservers() {
        observers.stream().forEach(it -> it.update());
    }
}

abstract class StatusObserver {

    private StatusSubject subject;

    public StatusObserver(StatusSubject subject) {
        this.subject = subject;
        subject.add(this);
    }

    public StatusSubject getSubject() {
        return subject;
    }

    public abstract void update();
}

class PublishMessageToQueue extends StatusObserver {

    public PublishMessageToQueue(StatusSubject subject) {
        super(subject);
    }

    @Override
    public void update() {
        System.out.println(getClass().getSimpleName() + " updating...");
    }
}

class UpdataUIStatusBarObserver extends StatusObserver {

    public UpdataUIStatusBarObserver(StatusSubject subject) {
        super(subject);
    }

    @Override
    public void update() {
        System.out.println(getClass().getSimpleName() + " updating...");
    }
}
```

Here is how to construct the observer and notify observers. 

```
StatusSubject subject = new StatusSubject();
new UpdataUIStatusBarObserver(subject);
new PublishMessageToQueue(subject);

subject.updateStatus(1);
subject.notifyObservers();
```

Here is the output. 

```
UpdataUIStatusBarObserver updating...
PublishMessageToQueue updating...
```



