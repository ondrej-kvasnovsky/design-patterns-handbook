# Too Many Returns

Too many returns \(accompanied with too many if branches\) lead to long, complex and hard to maintain methods.

### Example

Lets take this example of validator. It takes an object and validates its properties. As the object grows, the method grows. As the validation logic grows, the method grows and gets more and more complex. We can already see multiple inner `if`s.

```
const Validator = require('validator')
const _ = require('lodash')

module.exports = class {
  validate({imageUrl, proxy, useProxy, width, height, keepRatio, quality}) {
    if (!Validator.isURL(imageUrl) && !imageUrl.startsWith('data:image')) {
      return 'imageUrl needs to be a valid URL'
    }

    if (proxy && useProxy) {
      if (!Validator.isURL(proxy)) {
        return 'proxy needs to be a valid URL'
      }
      if (!_.isBoolean(useProxy)) {
        return 'useProxy needs to be a boolean'
      }
    }

    if (!_.isNumber(width)) {
      return 'width needs to be a number'
    }

    if (!_.isNumber(height)) {
      return 'height needs to be a number'
    }

    if (!_.isBoolean(keepRatio)) {
      return 'keepRatio needs to be a boolean'
    }

    if (quality) {
      if (!_.isNumber(quality)) {
        return 'quality needs to be a number'
      }
    }
  }
}

```

Instead of too many `if` branches each with its own `return` statement, we can create an object that will avoid that. 

```
const Validator = require('validator')
const _ = require('lodash')

function validate({imageUrl, proxy, useProxy, width, height, keepRatio, quality}) {
  return new RequestValidator()
        .withUrl(imageUrl)
        .withProxy(proxy, useProxy)
        .withWidth(width)
        .withHeight(height)
        .withKeepRatio(keepRatio)
        .withQuality(quality)
        .validate()
}

module.exports = validate

class RequestValidator {
  constructor() {
    this.errors = []
  }

  withUrl(imageUrl) {
    if (!Validator.isURL(imageUrl) && !imageUrl.startsWith('data:image')) {
      this.errors.push('imageUrl needs to be a valid URL')
    }
    return this
  }

  withProxy(proxy, useProxy) {
    if (proxy && useProxy) {
      if (!Validator.isURL(proxy)) {
        this.errors.push('proxy needs to be a valid URL')
      }
      if (!_.isBoolean(useProxy)) {
        this.errors.push('useProxy needs to be a boolean')
      }
    }
    return this
  }

  withWidth(width) {
    if (!_.isNumber(width)) {
      this.errors.push( 'width needs to be a number')
    }
    return this
  }

  withHeight(height) {
    if (!_.isNumber(height)) {
      this.errors.push( 'height needs to be a number')
    }
    return this
  }

  withKeepRatio(keepRatio) {
    if (!_.isBoolean(keepRatio)) {
      this.errors.push( 'keepRatio needs to be a boolean')
    }
    return this
  }

  withQuality(quality) {
    if (quality && !_.isNumber(quality)) {
      this.errors.push( 'quality needs to be a number')
    }
    return this
  }

  validate() {
    if (!_.isEmpty(this.errors)) {
      return _.join(this.errors, ', ')
    }
  }
}
```



