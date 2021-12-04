# RxJS notes

Notes from: https://egghead.io/lessons/rxjs-thinking-reactively-with-rxjs-course-intro

When using RxJS is a good solution?
Problem involves:

- passage of time (async operations, wait 3s before HTTP request)
- coordination of multipe types of events (ex. wait user to click, make request, redirect to account page)

A lot of FE problems match this: calls to database in background, user performing many weird actions in the app

Future requirements might build up, increasing our technical debt.

## Async problems

- Race conditions
- Memory leaks
- Complex state machines
- Uncaught async errors

## Observable

Observable represents a stream or source of data that can arrive over time. You can create them from anything but mostly from events. By default observables are "cold" - they don't activate a producer until there's a subscription.

Tap of water = observable
Turn the handle = subscription

In subscriptions we react to the events.

Subscription is an object that has unsubscribe method, so that we can unsubscribe later on.
We can also pass a function that will handle errors and completion of events

```js
const subscription = myObservable.subscribe({
  // on successful emissions
  next: (event) => console.log(event),
  // on errors
  error: (error) => console.log(error),
  // called once on completion
  complete: () => console.log("complete!"),
});
```

By default, a subscription creates a one on one, one-sided conversation between the observable and observer. This is like your boss (the observable) yelling (emitting) at you (the observer) for merging a bad PR.

AKA unicast

They can be multicast (speaker at the conference, talking to an audience) when used ex. with `Subjects`

Observable doesn't care about what happends with the data. It just pushes it down the line to the subscription.

## Operations

"Lodash for events"
They offer a way to manipulate data from the source - returning another observable

Operators exist within a pipe. It works like an assembly line that takes values from Observable through provided operators.
