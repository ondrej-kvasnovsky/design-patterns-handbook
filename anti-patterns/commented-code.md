

# Commented Code

If we fail to create meaningful code with meaningful names, we tend to create comments to explain our intentions and give other people clues. 

It might seem, that better no comment than comment that is false. The comments can get old because automated refactoring can't fix all the comments.  

```
Map<String, Object> result = new HashMap<>();
if (numItems < 0) {
  numItems = 0; // use -1 to get all without limit
} else if (numItems == 0) {
  numItems = 10; // default to max of 20 items in a list
}
```



