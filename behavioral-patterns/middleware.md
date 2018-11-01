# Middleware

Middleware pattern is used to handle a request and a response in code \(in pipe-line-like manner\). It can decide whether to pass execution to next middleware, it can call its own code before or after it passes execution to the next one. The middleware do not know about each other.

Middleware is created from Chain of Responsibility and Intercepting Filter patterns.

Usually, a message, context or request and response are send to each middleware function. Then it is mutated or cloned and then mutated. That way, we can extend code by adding a new middleware.

## Example

We are going to create a http server that will have an ability to add middle-wares. The main reason of this code is to show how middle wares can be implemented.

    const http = require('http');

    // wrapper to easier start of http server
    class Server {
      async start(port, requestHandler) {
        const server = http.createServer(requestHandler);
        server.listen(port, (err) => {
          if (err) {
            return console.log('Error', err)
          }
          console.log(`Server is listening on ${port}`)
        });
      }
    }

    // class to keep middle-wares in a linked list
    class Node {
      constructor(value) {
        this.value = value;
        this.next = null;
      }
    }

    class Balast {
      constructor() {
        this.middlewares = { // linked list of middle wares
          head: undefined,
          tail: undefined
        };
      }

      // adds new middleware
      use(middleware) {
        const newNode = new Node(middleware);
        if (this.middlewares.head === undefined) {
          this.middlewares.head = newNode;
        } else {
          if (this.middlewares.tail) {
            this.middlewares.tail.next = newNode;
          } else {
            this.middlewares.head.next = newNode;
            this.middlewares.tail = newNode;
          }
        }
      }

      // triggers chain of middle-wares
      async serve(request, response) {
        const current = this.middlewares.head;
        if (current) {
          const nextOne = current.next || (() => {
          });
          await current.value(request, response, nextOne);
        }
      }

      // starts a server and registers handler that triggers chain of middle-wares
      async listen(port) {
        const requestHandler = async (request, response) => {
          console.log('request handler: start');
          await this.serve(request, response);
          await response.end(null);
          console.log('request handler: done');
        };
        await new Server().start(port, requestHandler);
      }
    }

    const balast = new Balast(); // just random name of our http server
    const parseToJson = async (request, response, next) => {
      request.myData = {some: 'json value'};
      console.log('parseToJson', request.myData);
      await next.value(request, response, next.next);
    };
    balast.use(parseToJson);

    const enhanceMyData = async (request, response, next) => {
      request.myData.enhancedField = 123;
      console.log('enhanceMyData', request.myData);
      await next.value(request, response, next.next);
    };
    balast.use(enhanceMyData);

    const stringifyData = async (request, response, next) => {
      let content = JSON.stringify(request.myData);
      console.log("stringifyData", content);
      await next.value(request, response, next.next);
    };
    balast.use(stringifyData);

    const writeData = async (request, response, next) => {
      let content = JSON.stringify(request.myData);
      console.log("writeData", content);
      await response.write(content);
    };
    balast.use(writeData);

    const port = 3000;
    Promise.resolve(balast.listen(port));




