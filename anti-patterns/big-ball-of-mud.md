# Big Ball of Mud

Big Ball of Mud anti pattern, first described by [Brian Foote and Joseph Yoder](https://en.wikipedia.org/wiki/Big_ball_of_mud), is result of under-engineering. It is described as:

* everything talks to everything
* data are shared without limitations
* meaningless or misleading naming
* functions are not atomic and they have many responsibilities
* code is duplicated
* difficult to follow the code flow
* not clear intent of the code
* code is patched multiple times without refactoring

Here are some root causes of Big Ball of Mud project:

* Lack of upfront design
* Late changes to the requirements
* Late changes to the architecture
* Piecemeal growth

If we ever get to work with Big Ball of Mud, we need to follow the Boy Scouts of America.

`Leave the campground cleaner than you found it.`

But before we can follow that rule, we need to get understanding of that code, which is the most difficult thing to do on that kind of project.

### Example of Big Ball of Mud pattern

How does a big ball of mud looks? Lets try to create a concise example of Big Ball of Mud. The following code demonstrates the  feelings we get when we start reading Big Ball of Mud code. We don't know where it starts, we don't what it actually does, we don't know why it does certain things and we can see there are confusing comments that were added as requirements changed. Also we can see there was never any design applied on that code and certainly, no refactoring was done, like never ever. One could doubt that this code passed a code review.

```
public class Executor {

    public UKF data;
    public RenderHtml service;

    public Executor(UKF data, RenderHtml service) {
        this.data = data;
        this.service = service;
    }

    public String makeData() {
        String show = service.show();
        System.out.println(show);
        // need to return empty string or it fails in other services
        return "";
    }

    public String makeOtherData() {
        String show = service.show();
        System.out.println(show);
        // need to return empty string or it fails in other services
        return "";
    }
}
```

```
public class RenderHtml {

    public static UKF ukf = new UKF();

    public static String show() {
        String input = "";
        // TODO: refactor this, probably shouldn't use toString
        input += ukf.toString();
        return input;
    }
}
```

```
public class UKF {
    private String u;
    private Integer k;

    public void doIt() {
        Executor executor = new Executor(new UKF(), new RenderHtml());
        String s = executor.makeData();
        RenderHtml.ukf.u = s;
        // JIRA-123 changed value to 100 because otherwise it makes no sense!
        RenderHtml.ukf.k = 100;
    }
}
```

Image these classes are used by many other classes in our application. Would you dare to change `RenderHtml.ukf.k = 100;` to `RenderHtml.ukf.k = 101;`? If yes, why? If no, why? Nobody can tell, that is the effect of Big Ball of Mud pattern. It results in code nobody wants to change.

