# If-else over ternary operator

We need to avoid very long lines that are using ternary operator, because it makes code very difficult to read. 

Here is an example, last line in a controller that decides what is going to be returned.

```
return result != null ? ResponseEntity.ok(result).header("Location", "/v1/task/" + result.getId()).build() : Response.status(Status.BAD_REQUEST).header("ErrorMessage", "Invalid taskdata").build();
```



