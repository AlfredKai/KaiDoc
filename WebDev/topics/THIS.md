# THIS

[this in MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)

## Global context

In the global execution context (outside of any function), `this` refers to the global object whether in strict mode or not.

>You can always easily get the global object using the global [globalThis](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis) property, regardless of the current context in which your code is running.

## Function context

Inside a function, the value of this depends on how the function is called.

### Simple call

- not in strict mode: `this` will default to the global object, which is `window` in a browser.
- strict mode: if the value of `this` is not set when entering an execution context, it remains as `undefined`.

### call(), apply()

To set the value of this to a particular value when calling a function, use `call()`, or `apply()`.

>Note that in non–strict mode, with `call` and `apply`, if the value passed as `this` is not an object, an attempt will be made to convert it to an object using the internal `ToObject` operation. So if the value passed is a primitive like `7` or `'foo'`, it will be converted to an Object using the related constructor, so the primitive number `7` is converted to an object as if by `new Number(7)` and the string 'foo' to an object as if by `new String('foo')`.

### bind()

ECMAScript 5 introduced `Function.prototype.bind()`. Calling `f.bind(someObject)` creates a new function with the same body and scope as `f`, but where `this` occurs in the original function, in the new function it is permanently bound to the first argument of `bind`, regardless of how the function is being used.

### Arrow functions

In [arrow functions](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Functions/Arrow_functions), `this` retains the value of the enclosing lexical context's `this`. In global code, it will be set to the global object.

No matter what, arrow functions' `this` is set to what it was when it was created. The same applies to arrow functions created inside other functions: their `this` remains that of the enclosing lexical context.

### As an object method

When a function is called as a method of an object, its `this` is set to the object the method is called on.

>Note that this behavior is not at all affected by how or where the function was defined. The `this` binding is only affected by the most immediate member reference.

### `this` on the object's prototype chain

The same notion holds true for methods defined somewhere on the object's prototype chain. If the method is on an object's prototype chain, `this` refers to the object the method was called on, as if the method were on the object.

### `this` with a getter or setter

Again, the same notion holds true when a function is invoked from a getter or a setter. A function used as getter or setter has its `this` bound to the object from which the property is being set or gotten.

### As a constructor

When a function is used as a constructor (with the `new` keyword), its `this` is bound to the new object being constructed.

>While the default for a constructor is to return the object referenced by `this`, it can instead return some other object (if the return value isn't an object, then the `this` object is returned).

### As a DOM event handler

When a function is used as an event handler, its `this` is set to the element the event fired from (some browsers do not follow this convention for listeners added dynamically with methods other than `addEventListener()`).

### In an inline event handler

When the code is called from an inline [on-event handler](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Event_handlers), its `this` is set to the DOM element on which the listener is placed.