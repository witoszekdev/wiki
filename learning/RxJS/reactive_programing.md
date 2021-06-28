Notes from https://gist.github.com/staltz/868e7e9bc2a7b8c1f754 and https://egghead.io/lessons/rxjs-understand-reactive-programming-using-rxjs

# Introduction to Reactive Programming

Array items live inside computer memory, streams don't have this property and arrive over time.
We can apply functional programming aproach to both of them (using methods like `map`, `filter` and `reduce`)

To understand streams better we can use [Marble Diagrams](https://rxmarbles.com/) also written in an ASCII form like this:

```
  clickStream: ---c----c--c----c------c-->
               vvvvv map(c becomes 1) vvvv
               ---1----1--1----1------1-->
               vvvvvvvvv scan(+) vvvvvvvvv
counterStream: ---1----2--3----4------5-->
```

Why use event streams?

> Specify dynamic behavior of a value at the time of its creation.

```js
var a = 3;
var b = 10 * a;

console.log(b); // 30

a = 4;
b = 11 * a; // oopsie!
console.log(b); // 44
```

In this code we want to specify the behavior of `b` - it should always be the `10 * a`. But changing `a` later on doesn't change the `b` value. Insteaad we need to assign new value again. This leads a way for a mistake like in our code.

_Stream example_

```js
var streamA = Rx.Observable.of(3);
var streamB = streamA.map((a) => 10 * a);
streamA.set(4);
streamB.subscribe((b) => console.log(b)); // 30
```

This won't work

**The behavior of values can be specified ONLY at the time of declaration**

So we need to specify that the value of A can change over time:

```js
var streamA = Rx.Observable.of(3, 4);
```

## Making requests

Fun fact: _Promises_ are basically a **simplified version of an event stream** that can have only one value or only one event, which is either the resolved value or an error.

If we want to make a request we have to map over the values from requestStream (containing value of our endpoint url) with another observable.

```js
var responseStream = requestStream
	.map(requestUrl =>
		Rx.Observable.fromPromise(jQuery.getJSON(requestUrl));
	);
```

This creates an Observable of an Observable, aka **meta stream**.

![map](https://d2eip9sf3oo6c2.cloudfront.net/asciicasts/Introduction%20to%20Reactive%20Programming/original_rxjs-use-rxjs-async-requests-and-responses/rxjs-use-rxjs-async-requests-and-responses-marble-diagram.png?1501285136)

Instaed what we want is to have one stream that has the value from request. To do that we have to use `flatMap`

![flatMap](https://d2eip9sf3oo6c2.cloudfront.net/asciicasts/Introduction%20to%20Reactive%20Programming/original_rxjs-use-rxjs-async-requests-and-responses/rxjs-use-rxjs-async-requests-and-responses-flat-marble-diagram.png?1501285135)

### Shared requests

When we subscribe to one stream and it uses `map` on other stream it creates chain of subscriptions: `stream1 <==> stream2 <==> subscriber`.

In order to avoid this behavior we can make the `stream1` shared by using `.shareReplay(1)`. When 1 subscriber subscribes to this stream all other subscribers that will subscribe later will share the same subscription. It will also replay the responses if new subscribers would connect over time.

### Using cached data

In order to use latest value from other stream when handling events in another stream (ex. pick different user from a response when clicked on a button) we can use `withLatestFrom`
