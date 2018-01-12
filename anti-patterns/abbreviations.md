# Abbreviations

Abbreviations in code are causing higher cognitive load for developers. They should be avoided. There is always a better name to discover.

Look at `numItems` method parameter. `numItems` can mean couple of things - number of items, numerical items or number items. But in reality, it is limit that specifies maximum number of items that are requested. We would do better job if we just stick to `limit` as the name for that variable.

```
Response listJobs(@RequestParam("offset") final int startsWith, @RequestParam("limit") int numItems)
```



