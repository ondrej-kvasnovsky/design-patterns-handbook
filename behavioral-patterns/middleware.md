# Middleware

Middleware pattern is used to handle a request and a response in code \(in pipe-line-like manner\). It can decide whether to pass execution to next middleware, it can call its own code before or after it passes execution to the next one. The middleware do not know about each other. 

Middleware is created from Chain of Responsibility and Intercepting Filter patterns. 

Usually, a message, context or request and response are send to each middleware function. Then it is mutated or cloned and then mutated. That way, we can extend code by adding a new middleware. 

## Example



