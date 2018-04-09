A collection of various topics related to JavaScript.

## Functions (Built-in)

### setTimeout
setTimeout is asynchronous, in that it queues the function reference it receives to run once the current call stack has finished executing. It doesn’t however, execute concurrently, or on a separate thread (due to JavaScript’s single-threaded nature). In simple terms, it defers the execution of its code until after the rest of the call stack is completed. The amount of time provided to a setTimeout, e.g. 1000ms, is only the minimum amount of time the function will wait to execute, not the exact amount.
```javascript
console.log(1);
setTimeout(function(){
  console.log(2);
  }, 0);
console.log(3);
// Outputs: 1, 3, 2
```


## General Topics

### Hoisting
- refers to the movement of declaration of variables and functions to the top of their scope and their lower declarations are modified and re-written as simple assignments, something automatically done when JS code is parsed
- mostly done with “var”-type variables
- can result in uncaught ‘undefined’ errors because the JS engine knows the variable exists but does not yet have a value
- if there was no declaration, a ReferenceError would be thrown
- play it safe and define variables at top of scopes
- function declarations are usually safe and will be hoisted

### Refs
- refs are used to get a reference to a DOM node or instance of a React component
  - if it points to a DOM node, use ‘this.refs.ref’ to retrieve it
  - if it points to a custom React component you’ve made yourself, need to use ‘ReactDOM.findDOMNode(this.refs.ref)’
- in the lifecycle methods, ref is set after render but before componentDidMount()
- string refs (ref=“string”) will likely be deprecated in the future
- the good use cases for refs:
  - managing focus, text selection or media playback
  - triggering imperative animations
  - integrating with 3rd party DOM libraries

### Arguments in JS Functions
If a wrong number of arguments is passed to a JS function, nothing will happen- meaning you won't get an error or a warning as passing the parameters in JS is optional. All the parameters that weren't "supplied" will have the undefined value.

### NaN in JavaScript:
- a property of the global object
- it is the returned value when a Math function fails or when a function trying to parse a number fails
- Use Number.isNaN() or isNaN() to most clearly determine whether a value is NaN
- By definition, NaN is the return value from operations which have an undefined numerical result. Hence why, in JavaScript, aside from being part of the global object, it is also part of the Number object: Number.NaN. It is still a numeric data type, but it is undefined as a real number.

### Difference Between event.currentTarget and event.target
- CurrentTarget identifies the current target for the event, as the event traverses the DOM. It always refers to the element to which the event handler has been attached, as opposed to event.target which identifies the element on which the event occurred
- Example: e.target keep an object that trigger 'click' event - <li>. In it's turn, the e.currentTarget refers to the element to which handler was attached. It's mean that e.currentTarget === <ul> === this

### The JavaScript Event Loop
The browser JS execution (event loop) consists of heap (memory), call stack of functions (FILO), WebAPI’s such as setTimeouts, AJAX calls, and the DOM and a callback queue, which executes callbacks AFTER the call stack is emptied. JS is ‘single-threaded’, which means it can only execute one thing at a time.

### JS Prototypal Inheritance
- JS has prototypal inheritance for its objects instead of class-based inheritance languages like C#, Java, or Ruby. When creating a new object (class), can define properties and methods in the parent class that every child object instance will also have in their definition. To create smaller child objects, can define methods on the parent as {objectName}.prototype.{methodName} outside of the parent object definition. Child objects will not have this method defined inside them, instead, when the code is run, it will search up the inheritance chain (after it check’s the child’s own prototype) and look for the method in an ancestor’s definition and then apply it to the child.
- apply() and call() are identical but apply requires an array of arguments to be passed as the second arg while call takes each argument individually. In ES6, can use spread operator for the call function to pass an array of args.
  - e.g. theFunction.apply(obj, [arg1, arg2]), theFunction.call(obj, arg1, arg2), theFunction.call(obj, …[arg1, arg2]
- 3 steps to set up inheritance chain for objects:
  1. in child constructor, use Parent.call(this); to get access to the parent’s defined methods and properties
  2. outside of child definition, use Child.prototype = Object.create(Parent.prototype); which sets the Parent as the child’s prototype
  3. last, to set up the constructor, use Child.prototype.constructor = Child;

### Difference between “=“, “==“, and “===“ in JS:
- “=“ is the assignment operator, ie assigning a value to a variable
- “==” is the non-strict equality operator, meaning it does a data type conversion of one side to the other side’s data type to evaluate if both sides have the same value
- “===“ is the strict equality operator, meaning both sides must be of the same data type and same value to output true

### Difference between const, var, and let in JS:
- const is used for constant variables, ie they cannot be re-assigned
- var is meant for global variables (function scope), where the declaration of the variable is hoisted to the top, before the initial assignment. No error will be thrown as the variable will be returned as undefined
- let variables are only available in the context of their block (meant to be used in blocks). The variable will not be hoisted and a ReferenceError will be thrown.


## Main New Features in ES7 (ECMAScript 2016)

### Object Spread / Rest Properties
- With the changes in Object Rest Properties, you can now get the “rest of the properties” from an object, when doing de-structuring assignment, as a new object.
```javascript
var {a, b, c, ...x} = {a: 1, b: 2, c: 3, x: 4, y: 5, z: 6};
console.log(a) = 1
console.log(b) = 2
console.log(c) = 3
console.log(x) = {x: 4, y: 5, z: 6}
```
- The Object Spread property re-structures multiple objects into a new object. Note that if there are multiple assignments to the same variable, the last one written will be the defined one and will overwrite previous definitions.
```javascript
var a = 1, b = 2, c = 3;
var x = {x: 4, y: 5, z: 6};
var obj = {a, b, c, …x}
console.log(obj) // {a: 1, b: 2, c: 3, x: 4, y: 5, z: 6}
```

### Observables
The Observable type can be used to model push-based data sources such as DOM events, timer intervals, and sockets. Using the Observable constructor, we can create a function which returns an observable stream of events for an arbitrary DOM element and event type. The Observable type represents one of the fundamental protocols for processing asynchronous streams of data. It is particularly effective at modeling streams of data which originate from the environment and are pushed into the application, such as user interface events. By offering Observable as a component of the ECMAScript standard library, we allow platforms and applications to share a common push-based stream protocol. , the API standard for an observable includes a method to remove or unregister the events / stream from the underlying code. This means you now have a common way to clean up your event handlers, no matter where the events are coming from. No more memory leaks due to mismatched API design or forgetting to unregister your handlers (unless you don’t implement the method call in your observable, of course). This can be used with the built-in unsubscribe() method.
- In addition, observables are:
  - Compositional: Observables can be composed with higher-order combinators.
	- Lazy: Observables do not start emitting data until an observer has subscribed.

### Await / Async
By adding “async” to the outer function definition, you can now use the “await” keyword to call your other async functions. The functions that are called with “await” must also be marked as “async”, and there’s one more key to making them work: promises. By returning a promise from an async function, it can now be consumed with the “await” keyword, as shown above. The end result is code that is far easier to read and understand, easier to maintain, and easier to deal with as a developer. The code also now has the appearance of being synchronous. Await / Async also has the try { } and catch { } blocks to make error handling more simplified.

Sources: https://derickbailey.com/2017/06/06/3-features-of-es7-and-beyond-that-you-should-be-using-now/
