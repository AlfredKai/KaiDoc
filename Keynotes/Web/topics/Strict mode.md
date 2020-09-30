# Strict mode

[Strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)

Strict mode makes several changes to normal JavaScript semantics:

1. Eliminates some JavaScript silent errors by changing them to throw errors.
2. Fixes mistakes that make it difficult for JavaScript engines to perform optimizations: strict mode code can sometimes be made to run faster than identical code that's not strict mode.
3. Prohibits some syntax likely to be defined in future versions of ECMAScript.

## Invoking strict mode

- Strict mode for scripts
- Strict mode for functions
- Strict mode for modules
  - The entire contents of JavaScript modules are automatically in strict mode, with no statement needed to initiate it.

## Changes in strict mode

Strict mode changes both syntax and runtime behavior. Changes generally fall into these categories:

- changes converting mistakes into errors (as syntax errors or at runtime)
- changes simplifying how the particular variable for a given use of a name is computed
- changes simplifying `eval` and `arguments`, changes making it easier to write "secure" JavaScript, and changes anticipating future ECMAScript evolution.

### Converting mistakes into errors

- prevent create global variables accidentally.
- illegal assignment. ex: assignment to `NaN`, non-writable global or property
- delete undeletable properties
- duplicate property names
- requires that function parameter names be unique
- forbids octal syntax
- forbids setting properties on primitive values

### Simplifying variable uses

Strict mode simplifies how variable names map to particular variable definitions in the code. Many compiler optimizations rely on the ability to say that variable X is stored in that location: this is critical to fully optimizing JavaScript code. JavaScript sometimes makes this basic mapping of name to variable definition in the code impossible to perform until runtime. Strict mode removes most cases where this happens, so the compiler can better optimize strict mode code.

- prohibits `with`
- `eval` of strict mode code does not introduce new variables into the surrounding scope
- forbids deleting plain names

### Making `eval` and `arguments` simpler

- the names eval and arguments can't be bound or assigned in language syntax
- `arguments` objects for strict mode functions store the original arguments when the function was invoked
- `arguments.callee` is no longer supported

### "Securing" JavaScript

- the value passed as `this` to a function in strict mode is not forced into being an object (a.k.a. "boxed"). for a strict mode function, the specified `this` is not boxed into an object, and if unspecified, `this` will be `undefined`
- when a function `fun` is in the middle of being called, if `fun` is in strict mode, both `fun.caller` and `fun.arguments` are non-deletable properties which throw when set or retrieved
- `arguments` for strict mode functions no longer provide access to the corresponding function call's variables `arguments.caller`

### Paving the way for future ECMAScript versions

- in strict mode a short list of identifiers become reserved keywords. These words are `implements`, `interface`, `let`, `package`, `private`, `protected`, `public`, `static`, and `yield`
- Two Mozilla-specific caveats
  - if your code is JavaScript 1.7 or greater (for example in chrome code or when using the right `<script type="">`) and is strict mode code, `let` and `yield` have the functionality they've had since those keywords were first introduced. But strict mode code on the web, loaded with `<script src="">` or `<script>...</script>`, won't be able to use `let/yield` as identifiers
  - ES5 unconditionally reserves the words `class`, `enum`, `export`, `extends`, `import`, and `super`, before Firefox 5 Mozilla reserved them only in strict mode
- strict mode prohibits function statements not at the top level of a script or function