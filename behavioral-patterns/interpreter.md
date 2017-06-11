# Interpreter 

* Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language.
* Map a domain to a language, the language to a grammar, and the grammar to a hierarchical object-oriented design.

### Example - Expression with variables

We are going to use interpreter to evaluate mathematical expression with variables. 

```
class Evaluator {
    private Expression syntaxTree;

    public Evaluator(String expression) {
        Stack<Expression> expressionStack = new Stack<>();

        for (String token : expression.split(" ")) {

            if (token.equals("+")) {
                Expression left = expressionStack.pop();
                Expression right = expressionStack.pop();
                Expression subExpression = new Plus(left, right);
                expressionStack.push(subExpression);
            } else if (token.equals("-")) {
                Expression right = expressionStack.pop();
                Expression left = expressionStack.pop();
                Expression subExpression = new Minus(left, right);
                expressionStack.push(subExpression);
            } else if (Character.isDigit(token.charAt(0))) {
                expressionStack.push(new Number(Integer.valueOf(token)));
            } else {
                expressionStack.push(new Variable(token));
            }
        }
        syntaxTree = expressionStack.pop();
    }

    public int interpret(Map<String, Expression> variables) {
        return syntaxTree.intepret(variables);
    }
}

interface Expression {
    int intepret(Map<String, Expression> variables);
}

class Number implements Expression {

    private Integer number;

    public Number(Integer number) {
        this.number = number;
    }

    @Override
    public int intepret(Map<String, Expression> variables) {
        return number;
    }
}

class Plus implements Expression {

    private Expression left;
    private Expression right;

    public Plus(Expression left, Expression right) {
        this.left = left;
        this.right = right;
    }

    @Override
    public int intepret(Map<String, Expression> variables) {
        return left.intepret(variables) + right.intepret(variables);
    }
}

class Minus implements Expression {

    private Expression left;
    private Expression right;

    public Minus(Expression left, Expression right) {
        this.left = left;
        this.right = right;
    }

    @Override
    public int intepret(Map<String, Expression> variables) {
        return left.intepret(variables) - right.intepret(variables);
    }
}

class Variable implements Expression {

    private String name;

    public Variable(String name) {
        this.name = name;
    }

    @Override
    public int intepret(Map<String, Expression> variables) {
        return variables.get(name).intepret(variables);
    }
}
```

We define the math formula using [polish reversed notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation): 52 1 2 + -.  It is going to return 49.

```
// reverse polish notation
String expression = "52 1 2 + -";
Evaluator evaluator = new Evaluator(expression);

// variables is the context
Map<String, Expression> variables = new HashMap<>();
variables.put("w", new Number(5));
variables.put("x", new Number(10));
variables.put("z", new Number(25));

int result = evaluator.interpret(variables);
System.out.println(result);
```



