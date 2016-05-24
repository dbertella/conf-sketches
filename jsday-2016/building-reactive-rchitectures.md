# Building Reactive Architectures | Matthew Podwysocki

## Netflix
Massive query and massive linking across user and data

@ReactWindows

@ReactiveX

Async is awful

- Living in Callback Hell
- Events and state are mess

**Promise are only part of solution**

There is no way to cancel a promise, Fetch API doesn't have an abort method.

Callback hell solve in the reactive way!

## Reactive programming

- React to load
- React to failure
- React to users

What's the difference between iterable and events? There is no difference

Everything is a stream.

**You must flatMap it.**

### story on reactive programming
- jquery: wrong
- knockout or angular: 2 way data binding
- angular 2: implement observables
- ngrx: redux stores in observable way for angular 2
- cycle: unidirectional dataflow reactive programming
- react: subscribe and unsubscribe with rx extension in component mount and unmount

Observable coming to ES2017??
Possible standard implementation

Make reactive great again!
