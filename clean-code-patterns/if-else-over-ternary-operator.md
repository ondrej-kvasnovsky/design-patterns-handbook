# If-else over ternary operator

We need to avoid very long lines that are using ternary operator, because it makes code very difficult to read.

> Straightforward usage of ternary operator is OK, for example: String name = user ? user.getName\(\) : "";

Here is an example, last line in a controller that decides what is going to be returned.

```
return result != null ? ResponseEntity.ok(result).header("Location", "/v1/task/" + result.getId()).build() : Response.status(Status.BAD_REQUEST).header("ErrorMessage", "Invalid taskdata").build();
```

What if we want to refactor such a code? We first split it into if-else branches and then we figure out what is actually happening.

