# Revealing Constructor

Revealing constructor is used in JavaScript, for example, to implement `Promise` class. The constructor of `Promise` class takes function with two parameters \(`resolve` and `reject`\) into constructor. It is perfect way to hide implementation details and there is no way somebody can mess around with `resolve` and `reject` after the instance if created.

## Example - Read-only event emitter

The `emit` function of `ReadOnlyEmitter` can be called only in function that is passed into the constructor. 

```
const EventEmitter = require('events');

class ReadOnlyEmitter extends EventEmitter {
  constructor (executor) {
    super();
    const emit = this.emit.bind(this);
    this.emit = undefined; // hide emit function
    executor(emit);
  }
};

const ticker = new ReadOnlyEmitter((emit) => {
  let tickCount = 0;
  setInterval(() => emit('tick', tickCount++), 1000);
});

ticker.on('tick', (count) => console.log('Tick', count));
```



