# Generic Interface over Function

We should create generic interfaces only if they are adding value and increasing readability of the code.

```
export interface MessageTranslator<A, B> {
  to(from: A): B;
}
```

```
export class Processor {
  private detectIntentRequestTranslator: MessageTranslator<DetectIntentRequestDF, DetectIntentRequest>;
  private detectIntentResponseTranslator: MessageTranslator<DetectIntentResponse, DetectIntentResponseDF>;

  constructor() {
    this.detectIntentRequestTranslator = new DetectIntentRequestTranslator();
    this.detectIntentResponseTranslator = new DetectIntentResponseTranslator();
  }

  doSomething(): number {
    const request: DetectIntentRequest = this.detectIntentRequestTranslator.to(call.request);
    const resposne: DetectIntentResponse = this.detectIntentResponseTranslator.to(call.response);
    // do something
    return 1;
  }
}
```

The code above could look like this. We could just create the transformation functions and use them directly, because they are pure functions.

```
function toIntentRequest(request): DetectIntentRequest {
  return new DetectIntentRequest();
}

function toIntentResponse(request): DetectIntentResponse {
  return new DetectIntentResponse();
}

export class Processor {

  doSomething(): number {
    const request = toIntentRequest(call.request);
    const response = toIntentResponse(call.response);
    // do something
    return 1;
  }
}
```



