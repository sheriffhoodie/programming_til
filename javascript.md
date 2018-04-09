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
- The Observable type can be used to model push-based data sources such as DOM events, timer intervals, and sockets. Using the Observable constructor, we can create a function which returns an observable stream of events for an arbitrary DOM element and event type. The Observable type represents one of the fundamental protocols for processing asynchronous streams of data. It is particularly effective at modeling streams of data which originate from the environment and are pushed into the application, such as user interface events. By offering Observable as a component of the ECMAScript standard library, we allow platforms and applications to share a common push-based stream protocol. , the API standard for an observable includes a method to remove or unregister the events / stream from the underlying code. This means you now have a common way to clean up your event handlers, no matter where the events are coming from. No more memory leaks due to mismatched API design or forgetting to unregister your handlers (unless you don’t implement the method call in your observable, of course). This can be used with the built-in unsubscribe() method.
- In addition, observables are:
  - Compositional: Observables can be composed with higher-order combinators.
	- Lazy: Observables do not start emitting data until an observer has subscribed.

### Await / Async
  By adding “async” to the outer function definition, you can now use the “await” keyword to call your other async functions. The functions that are called with “await” must also be marked as “async”, and there’s one more key to making them work: promises. By returning a promise from an async function, it can now be consumed with the “await” keyword, as shown above. The end result is code that is far easier to read and understand, easier to maintain, and easier to deal with as a developer. The code also now has the appearance of being synchronous. Await / Async also has the try { } and catch { } blocks to make error handling more simplified.

Source: https://derickbailey.com/2017/06/06/3-features-of-es7-and-beyond-that-you-should-be-using-now/
