# Consistent Naming

The same things should be always use the same name in the code. Coding is not an writing exercise where we are asked to use as many synonym as possible to create high quality literature.

Look at that method variables. `offset` is renamed to `startsWith` and then renamed to `startFrom`. Then `limit` parameter is renamed to `numItems` and then `howMany` parameter. 

```
class Controller {
  private Service service;
  
  Response listJobs(@RequestParam("offset") final int startsWith, @RequestParam("limit") int numItems) {
    return Response.from(service.find(startsWith, numItems)).build();
  }
}

class Service {
  List<User> find(int startFrom, int howMany) {
     return null; // return something...
  }
}
```



