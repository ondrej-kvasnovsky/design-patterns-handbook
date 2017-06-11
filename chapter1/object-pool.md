# Object Pool

Object Pool is useful when creating a new instance costs a lot of time. We can make performance boost by keeping pool of reusable objects instead of making new objects on request.

### Example - Connection pool

Good example to explain object pool is a database connection pool.

```
class ConnectionPool {

    private Set<Connection> available = new HashSet<>();
    private Set<Connection> taken = new HashSet<>();

    public ConnectionPool() {
        available.add(new Connection());
        available.add(new Connection());
        available.add(new Connection());
    }

    public Connection getConnection() {
        Connection connection = available.iterator().next();
        available.remove(connection);
        taken.add(connection);
        return connection;
    }

    public void returnConnection(Connection connection) {
        taken.remove(connection);
        available.add(connection);
    }

    public String toString() {
        return "Available: " + available.size() + ", taken: " + taken.size();
    }
}

class Connection {
}
```

We can test the connection pool. We first create a new pool and then we obtain and return a connection.

```
ConnectionPool pool = new ConnectionPool();
System.out.println(pool);

Connection connection = pool.getConnection();
System.out.println(pool);

pool.returnConnection(connection);
System.out.println(pool);
```

Here is the output of our test.

```
Available: 3, taken: 0
Available: 2, taken: 1
Available: 3, taken: 0
```



