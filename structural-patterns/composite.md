# Composite

When ever we need to a tree structure, we use composite pattern. 

### Simple Composite

There is a way to create really simple tree structure using composite patter \(but we have to omit few classes in order to make it really simplistic\). 

```
class TreeNode {

    private String name;
    private List<TreeNode> children = new ArrayList<>();

    public TreeNode(String name) {
        this.name = name;
    }

    public void addChild(TreeNode child) {
        children.add(child);
    }
    
    public boolean isLeaf() {
        return children.size() == 0;
    }
}
```

Now we can create a tree using just TreeNode. 

```
TreeNode root = new TreeNode("Root");
root.addChild(new TreeNode("Node 1"));
root.addChild(new TreeNode("Leaf 1"));
```

### Composite with an interface

Here is an example of composite pattern. `Leaf` can't have any children. `Node` can contain multiple implementations of `Component` interface. Node class is enabling the tree structure.

```
interface Component {
    void sayYourName();
}

class Node implements Component {

    private String name;

    private List<Component> elements = new ArrayList<>();

    public Node(String name) {
        this.name = name;
    }

    @Override
    public void sayYourName() {
        System.out.println(name);
    }

    public void add(Component component) {
        elements.add(component);
    }
}

class Leaf implements Component {

    private String name;

    public Leaf(String name) {
        this.name = name;
    }

    @Override
    public void sayYourName() {
        System.out.println(name);
    }
}
```

Here is how to create a tree using the classes above. 

```
Node root = new Node("Root");
root.add(new Node("Node 1"));
root.add(new Leaf("Leaf 1"));
```

### Complex example with simple algorithm

A mathematical formula can be represented as a tree. Lets try it out and see how composite works in real life.

```
interface ArithmeticExpression {
    int evaluate();
}

class CompositeOperand implements ArithmeticExpression {

    private String operator;
    private List<ArithmeticExpression> expressions = new ArrayList<>();

    public CompositeOperand(String operator) {
        this.operator = operator;
    }

    public void add(ArithmeticExpression expression) {
        expressions.add(expression);
    }

    @Override
    public int evaluate() {

        List<Integer> evaluated = new ArrayList<>();

        for (ArithmeticExpression expression : expressions) {
            evaluated.add(expression.evaluate());
        }

        Integer result = null;
        for (Integer number : evaluated) {
            if (operator.equals("*")) {
                if (result == null) {
                    result = number;
                } else {
                    result *= number;
                }
            } else if (operator.equals("+")) {
                if (result == null) {
                    result = number;
                } else {
                    result += number;
                }
            } else if (operator.equals("-")) {
                if (result == null) {
                    result = number;
                } else {
                    result -= number;
                }
            }
        }

        return result;
    }
}

class NumericOperand implements ArithmeticExpression {

    private int number;

    public NumericOperand(int number) {
        this.number = number;
    }

    @Override
    public int evaluate() {
        return number;
    }
}
```

Now we can construct mathematical formula. We are going to use this `((7+3)*(5−2))`.

```
// create root
CompositeOperand multiply = new CompositeOperand("*");

// create +
CompositeOperand firstSum = new CompositeOperand("+");
firstSum.add(new NumericOperand(7));
firstSum.add(new NumericOperand(3));
multiply.add(firstSum);

// create -
CompositeOperand secondSum = new CompositeOperand("-");
secondSum.add(new NumericOperand(5));
secondSum.add(new NumericOperand(2));
multiply.add(secondSum);

// evaluate all children, then do math operation
int result = multiply.evaluate();
System.out.println(result);
```

The program will print out the following. 

```
30
```



