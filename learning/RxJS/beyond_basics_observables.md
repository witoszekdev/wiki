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
