# The Evolution of Asynchronous JavaScript | Alessandro Cinelli @cirpo

Asynchrony now and later


## Concurrency
Executing small pieces of code independently one from another.

JS running in a hosting environment.

JS is single thread, sort of concurrency, but hosting environment can use different thread

## Asynchrony

Async is great but can be hard
Human being has a sequential brain, we are not multi-tasker

Callbacks works well and you can solve the callback hell without being lazy.
Getting a value in a synchronous way is like pull a data from somewhere

Async callback is like push values

Error handling, Inversion of control, How can you tell it's async?

### Promises
Promise represent a proxy for a value, it associate hadlers on a single value.

ES2015 jobs -> free pass on the rollercoaster of event loop.
Mechanism inside js engine to go async

Promise are now the official way to provide async.

Object with three state Pending, fulfilled, rejected
- `.then()` method to be able to access to the value fulfilled
- `.catch()` method to handle Error

Control flow: solved
Inversion of control: solved
Error handling: solved
Async or sync: solved

But...

**Promise hell**

Don't use promise for controlling the flow.
#### Single Value
Single resolution is bad for streams, with promise you will handle a single value

#### Performances
Promises are slower than callback, but it's not gonna be an issue in 99,9% of the time.

### Generators
New type of function that doesn't have the run-to-completion behaviour of the other function.

The yeald is pausing the function! You have full controll on those functions.

Iterators are just one side, the other side is observables.

We can block, we can pull values, we can push values.

**Generators + Promises** for the win.

#### Problem, we need a library to run the generators.
**ES7 ES2016 => async / await** finally something inside the language itself

callbacks, then, async/await, highland (rx.js, streams)
