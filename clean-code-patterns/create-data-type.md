# Create Data Type

For more complex and growing systems, it is more readable and maintainable to create type instead of using too many parameters or maps. 

There is one exception. The exception is when we really need to handle dynamically changing number of parameters.

### Too many parameters

As the code grows, we can end up with methods that have too many parameters. 

```
class UserService {
    public void save(int id,
                     String fisrtname,
                     String lastname,
                     String street,
                     String city,
                     String apt,
                     String ssn,
                     boolean active,
                     boolean update) {
        // for example, it gets saved into database
    }
}
```

When we have so many parameters, we want to create an envelop data types that would wrap these objects. The easiest solution is to use a map, because it is provided by language itself. Lets see what is wrong with that. 

### Using Maps everywhere instead of classes

Lets look at an example. The controller gets some parameters. We have got only three and we could just pass them into service, imagine there 20 parameters that needs to be wrapped before they are sent to service. 

> It is questionable if using maps everywhere is wrong for dynamically typed languages. It depends, but many times, it is better to have data type for better maintainability.

```
class Controller {
    private Service service = new Service();
    public Map<String, Object> get(int id, String type, String name) {
        Map<String, Object> query = new HashMap<>();
        query.put("ID", id);
        query.put("TYPE", type);
        query.put("NAME", name);
        return service.find(query);
    }
}

class Service {
    public Map<String, Object> find(Map<String, Object> query) {
        HashMap<String, Object> result = new HashMap<>();
        int id = (int) query.get("ID");
        String type = (String) query.get("TYPE");
        String name = (String) query.get("NAME");
        // imagine we call some DAO to get data from database
        // String someData = dao.find(id, type, name);
        result.put("SOME_KEY", "some data from db");
        return result;
    }
}
```

What is wrong with using maps: 

* We are loosing information about type
* It is not clear what is the signature of the method, maps are too generic
* Keys in the maps are all around the code and it is difficult to do refactoring \(we can create helper class to store the keys, but it is just creating another class which shouldn't be even created\)

### Using Data Type instead of Maps or too many parameters

Lets create data types instead of using the maps or instead of having so many parameters. 

```
class Request {
    private final int id;
    private final String type;
    private final String name;

    public Request(int id, String type, String name) {
        this.id = id;
        this.type = type;
        this.name = name;
    }

    public int getId() {
        return id;
    }
    public String getType() {
        return type;
    }
    public String getName() {
        return name;
    }
}

class Result {
    private final String someData;

    public Result(String someData) {
        this.someData = someData;
    }

    public String getSomeData() {
        return someData;
    }
}

class Controller {
    private Service service = new Service();
    public Result get(int id, String type, String name) {
        Request request = new Request(id, type, name);
        return service.find(request);
    }
}

class Service {
    public Result find(Request query) {
        int id = query.getId();
        String type = query.getType();
        String name = query.getName();
        // imagine we call some DAO to get data from database
        // String someData = dao.find(id, type, name);
        return new Result("some data from db");
    }
}
```



