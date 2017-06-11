# Strategy

* Define the interface of an interchangeable family of algorithms
* Bury algorithm implementation details in derived classes
* Derived classes could be implemented using the Template Method patter
* Clients of the algorithm couple themselves strictly to the interface

### Example - Checking data providers

We are goint to use Strategy pattern to implement various strategies to check websites like Google, eBay and so on.

```
interface Strategy {
    void solve();
}

class GoogleStrategy implements Strategy {
    @Override
    public void solve() {
        System.out.println("Checking Google");
    }
}

class EbayStrategy implements Strategy {
    @Override
    public void solve() {
        System.out.println("Checking eBay");
    }
}

class StrategyExecutor {
    void execute() {
        Strategy[] strategies = {new GoogleStrategy(), new EbayStrategy()};
        Arrays.stream(strategies).forEach(Strategy::solve);
    }
}
```

Then we use the executor to run our strategies. 

```
new StrategyExecutor().execute();
```

Here is the output. 

```
Checking Google
Checking eBay
```



