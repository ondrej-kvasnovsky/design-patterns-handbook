# Mediator

Mediator provides way to implement many-to-many relationships between interacting peers. If peers interact directly between each other, it means they are tangled and we end up with spaghetti code. 

Imagine if all the airplanes would try to agree who is landing first between each other, rather than contacting the air traffic control tower. Mediator is the air traffic control tower.

### Example - Chat

User belongs to a group, if he wants to say something, he is always "redirected" to group. Group is the mediator, providing way users can talk together.

```
class Group {

    private List<User> users = new ArrayList<>();

    public void add(User user) {
        users.add(user);
    }

    public void say(String message, User author) {
        users.stream()
                .filter(user -> user != author)
                .forEach(user -> System.out.println(user.getName() + ": " + message));
    }
}

class User {

    private Group group;
    private String name;

    public User(Group group, String name) {
        this.group = group;
        this.name = name;
    }

    public Group getGroup() {
        return group;
    }

    public String getName() {
        return name;
    }

    public void say(String message) {
        group.say(message, this);
    }
}
```

Here is example of usage. 

```
Group group = new Group();

User john = new User(group, "John");
group.add(john);
User jimmy = new User(group, "Jimmy");
group.add(jimmy);
User emily = new User(group, "Emily");
group.add(emily);

jimmy.say("Hi everyone!");
```

When we run the code, Jimmy says "Hi everyone!" and users are notified with the new message.

```
John: Hi everyone!
Emily: Hi everyone!
```



