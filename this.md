## `this`

The value of `this` is defined based on how the method is called.

- Function
- Constructor
- Method
- .call() / .apply()

### Arrow functions and `this`
箭头函数中的 `this` 指向定义时所在的上下文（词法作用域）的 `this`，而不是调用时的对象。
Arrow functions create closures over the `this` value of the execution context.

[this in arrow functions - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this#this_in_arrow_functions)