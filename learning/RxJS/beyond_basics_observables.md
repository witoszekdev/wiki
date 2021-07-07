Notes from https://egghead.io/courses/rxjs-beyond-the-basics-creating-observables-from-scratch

# RxJS Beyond the Basics: Creating Observables from scratch

## Comparison between Obserable and function

**Obserables are generalization of a function**

_Lazy computation_ - we don't run the code unless we have to

```js
function foo() {
  console.log("hello");
  return 42;
}
```

this won't run unless we call the function

```js
console.log(foo());
// "hello"
// 42
```

The same happens with obserables - they won't run and return anything unless they're run

```js
var bar = Rx.Observable.create(function (observer) {
  console.log("Hello");
  observer.next(42);
});
```

nothing happends unless we subcribe:

```js
bar.subscribe(function (x) {
  console.log(x);
});
// 'Hello'
// 42
```

Observables are also synchronous just like functions are

_Function_

```js
console.log("before");
foo();
console.log("after");
// 'before'
// 'Hello'
// 42
// 'after'
```

_Obserable_

```js
console.log("before");
bar.subscribe(function (x) {
  console.log(x);
});
console.log("after");
// 'before'
// 'Hello'
// 42
// 'after'
```

### The difference

**Obserable can return many values by using `observer.next()` multiple times.**

```js
// inside obserable
observer.next(42);
observer.next(100);
observer.next(200);
```

This is run synchronously

```js
console.log("before");
bar.subscribe(function (x) {
  console.log(x);
});
console.log("after");
// "before"
// "Hello"
// 42
// 100
// 200
// "after"
```

**Obserable _can_ be asyncrhonous as well by emmiting the values over some time**

```js
// inside obserable
observer.next(42);
setTimeout(() => {
  observer.next(100);
}, 1000);

console.log("before");
bar.subscribe(/* ... */);
console.log("after");

// "before"
// => 42
// "after"
// 1000 seconds pass
// => 100
```

This runs long after we have run `.subscribe`

By calling function:

> Give me one value. Right. Now!

By calling `.subscribe`:

> Give me values... I dunno, when you have them. I'll wait :shrug:

## Generator functions

They are similar to Obserables: can return multiple values

Generator function allows to _pause_ the execution.
We also need to **explicitly** _pull the values out of generator_

```js
function* baz() {
  console.log("Hello");
  yield 42;
  yield 200;
}

const iterator = baz();
console.log(iterator.next().value);
// => "Hello"
// => 42
console.log(iterator.next().value);
// => 100
```

The values are not returned automatically we have to call `iterator.next()` to receive the value from generator

### The difference

Producer = Obserable
Consumer = `Obserable.subscribe()`

Producer = Generator function (`function* baz`)
Consumer = Iterator (`iterator.next()`)

In obserable the _Producer_ determines when the values are sent - **push**
In generator function the _Consumer_ determines when the values are sent - **pull**

### When to use which

Generator - passive generation of values, that can take long to retreive (ex. Fibbonaci sequence)
Obserables - sequence of "alive" values, like events, `setTimeout`, `setInterval`

## Throwing errors

We use `observer.error()` inside obserable

To handle those errors we provide a second function to the `subscribe` method:

```js
bar.subscribe(
  (x) => showValue(x),
  (err) => handleError(err)
);
```

It's a good idea to wrap the whole obserable in `try ... catch` block in order to catch all possible errors and push them to the subscriber

## Completion

The Obserable can also be "completed" by using `observer.complete()`. After that the Observable cannot emit any other value (they'll be ignored).

```js
var bar = Rx.Observable.create(function (observer) {
  try {
    observer.next(1);
    observer.next(2);
    observer.complete();
    setTimeout(function () {
      observer.next(3);
    }, 1000);
  } catch (err) {
    observer.error(err);
  }
});
```

When we listen you'll see that `3` is not displayed after 1 second because the observale is completed

To listen to the completion of obserable in the `subscribe` we can provide 3rd function:

```js
bar.subscribe(
  function nextValueHandler(x) {
    console.log(x);
  },
  function errorHandler(err) {
    console.log("Something went wrong: " + err);
  },
  function completeHandler() {
    console.log("done");
  }
);
```

[🔗 RxJS operators by category](http://reactivex.io/documentation/operators.html)

## RxJS - of operator

`of` operator replaces writing boilerplate of delivering synchronous values:

```js
var bar = Rx.Obserable.create((observer) => {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  observer.complete();
});
```

becomes

```js
var foo = Rx.Observable.of(1, 2, 3);
```

### fromArray

Does the same but accepts array instead of function arguments

```js
Rx.Observable.fromArray([1, 2, 3]);
```

### fromPromise

Takes only promise as an argument.
When the promise is resolved the obserable emits value, or emits an error. Then the obserable emits done event (Promise can be either resolved or rejected so it makes sense to complete - no other value will appear)

```js
Rx.Observable.fromPromise(fetch("https://null.jsbin.com"));
// Resolved => next + done
// Rejected => error + done
```

### from

Detects which from function to use automatically (fromArray, fromPromise).

```js
const obserable1 = Rx.Observable.from([1, 2, 3]);
const obserable2 = Rx.Observable.from(fetch("https://null.jsbin.com"));
```

We can also pass an iterator from generator function:

```js
function* generator() {
  yield 10;
  yield 20;
  yield 30;
}
const iterator = generator();
const obserable = Rx.Observable.from(iterator);

// next: 10, next: 20, next: 30
```

By doing that we convert from PULL to PUSH and therfore loose the ability to decite when we want to receive next value (everything in the generator is done synchronously)

### fromEventPAttern

Using this operator we can convert event handlers (`addEventHandler` and `removeEventHandler`) into observables. Those can be DOM events or Node.js events (what matters is this add and remove pattern)

```js
const obserable = Rx.Obserable.fromEventPattern(
  addEventHandler,
  removeEventHandler
);
// event happens => next: event
```

> Note: addEventHandler and removeEventHandler are functions that take a handler function and add an event listener to the DOM element using `.addEventListener(someElement, passedHandler)`

```js
function addEventHandler(handler) {
  document.addEventListener("click", handler);
}
```

#### How it works under the hook?

Under the hood fromEventPattern passes a handler to the `addEventHandler` we provide that will use `observer.next` to emit events when they happen:

```js
function fromEventPattern(add, remove) {
  return Rx.Obserable.create((observer) => {
    add((event) => observer.next(event));
  });
  // handle removal
}
```

### fromEvent

Specialized version of `fromEventPattern` that works with DOM events:

```js
Rx.Observable.fromEvent(target, eventType);
// example
const el = document.queryElement("#button");
Rx.Observable.fromEvent(el, "click");
```

## Boring operators

Shortcuts that allow us to skip writing simple obserables. They're used in combination with other operators.

### empty

Delivers just the observable completion.
Does nothing and tells you that it did nothing.

```js
const observable = Rx.Observable.empty();
// => done
```

Is eqivalent to:

```js
Rx.Obserable.create((observer) => {
  observer.complete();
});
```

### never

Creates an observable that returns _nothing_ at all.
Infinite obserable - the observer doesn't know if the value will come soon or not. The observer will infinitely wait for a value

```js
const obserable = Rx.Observable.never();
```

Is equivalent to:

```js
Rx.Obserable.create((observer) => {
  // nothing to see here folks!
});
```

### throw

Takes a JS error and simly emits obserable error

```js
const obserable = Rx.Obserable.throw(new Error("Oppsie doopsie!"));
```

Is equivalent to:

```js
Rx.Obserable.create((observer) => {
  observer.error(new Error("Oppsie doopsie!"));
});
```

## Time operators

### interval

An alternative to calling setInterval:

```js
const observale = Rx.Obserable.interval(1000);
```

Is equivalent to:

```js
const observable = Rx.Obserable.create((observer) => {
  let i = 0;
  setInterval(() => {
    observer.next(i);
    i += 1;
  }, 1000);
});
```

`interval` operator will create separate `setInterval` for each observer

```js
obserable.subscribe((x) => console.log(x));

setTimeout(() => {
  observable.subscribe((x) => console.log(x));
}, 1500);
//  0,  1,  0,  2, ...
// (1) (1) (2) (1)
// each observer receives separate values (not the same , at the same time!)
```

### timer

Works exactly as `interval` but waits for a start

**setTimeout + setInterval**

```js
const obserable = Rx.Obserable.timer(2000, 1000); // 2000 - how long to wait, 1000 - emit value every 1000 ms
// We can also pass a date
const date = new Date(new Date().getTime() + 3000); // in 3s from now
const obserable2 = Rx.Obserable.timer(date, 1000);
```

## How does creating obserable work?

`obserable.create` is the same as calling constructor on the obserable type:

```js
const obseravle = new Rx.Obserable(() => {});
```

The constructor accepts one argument - `subscribe` function

```js
function subscribe(observer) {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  observer.complete();
}

const observable = new Rx.Obserable(subscribe);
```

When we subscribe to the obserable we use `.subscribe()` method that accepts 3 arguments: next handler, error handler, complete handler.

```js
observable.subscribe(
  (x) => console.log(x), // next
  (err) => console.log(err), // error
  () => console.log("done") // complete
);
```

We can also subscribe by providing an object:

```js
obserable.subscribe({
  next: (x) => console.log(x),
  error: (err) => console.log(err),
  complete: () => console.log("done"),
});
```

This object can be extracted... let's call it `observer`

```js
const observer = {
  next: (x) => console.log(x),
  error: (err) => console.log(err),
  complete: () => console.log("done"),
};

obserable.subscribe(observer);
```

_HOLD UP_ it looks exactly like the method we're calling in the Obserable contructor function!

Yes. This all is just a convention. We can even get rid of the RxJS library:

```js
function subscribe(observer) {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  observer.complete();
}

const observer = {
  next: (x) => console.log(x),
  error: (err) => console.log(err),
  complete: () => console.log("done"),
};

subscribe(observer);
// does the same what the RxJS does
```

The `observable.subscribe` method normalizes function parameters into the `observer` object (with next, error, complete fields)

Why use the RxJS then? Because it provides nice helper functions. The rest is just JavaScript 😄

## Unsubscribing

If subscriber is not interested in the returned values anymore we can unsubscribe

```js
const subscription = observable.subscribe(/* ... */);
// after some time
subscription.unsubscribe();
```

We can also provide a "clear" function that will remove any side effects (just like `useEffect` React hook!)

```js
const observable = Rx.Obserable.create((observer) => {
  const id = setInterval(/* ... */);
  return () => {
    clearInterval(id);
  };
});
```

### How does it work under the hook?

Under the hood it works like that:

```js
function subscribe(observer) {
  const id = setInterval(/*  ...*/);
  return function unsubscribe() {
    clearInterval(id);
  };
}

const unsubscribe = subscribe({
  /* observer objec5 */
});
unsubscribe();
```

RxJS instead of returning just the `unsubscribe` function returns an `subscription` object that has `unsubscribe` function as on of methods
