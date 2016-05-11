# Functional Reactive programming with React.js by Luca Mezzalira

- Reactive programming
- Model view intent
- cycle.js

## Imperative, Functional, Reactive
- Imperative: Describing step by step what you want to achieve. Object has a state
- Functional: Manipulate data in the argument of the function, but you can't manipulate the intial data
- Reactive: Moving forward, used in backend development, in front end is quite new.

## Imperative vs Reactive
Reactive, less code, more readable.

- Observable pattern
- Iterator pattern

### [RxJs](http://reactivex.io/)
Start learning few concepts to understand reactive programming.

**streams**: sequence of ongoing events, everything can be a stream.
It can emit three different things value, error, or completed signal, (3 callbacks)

**cold observables**: waits until an observer subscribes to it before it begins to emit items
**hot observables**: emitting items as soon as it is created
**operators**

## Architectures timeline
- '80 MVC
- '90 MVP
- 2005 MVVM
- 2009 DCI (data context and interaction)
- 2013 FLUX
- 2015 MVI (Model view intent)

### MVI
You can't call a method from another module, they are separated, the only shared part of modules are observables.
Better separation of concerns, dependency injection, app easier to test, all the states inside models, the view is state free.
**View -> Render**
View is preparing the virtual tree
There is another library that will render the dom.

View is receiving data from model, pass them as output to the render

**Intent**
Input: raw input from user
Output to the model

**Model**
Input: data from the intent and store them

## Cycle.js
Your application is a pure function, that receive an input and return an Output

Core concepts:
- Object
- Functions
- Drivers
- Helpers
- Components

**Drivers** When you have to do something, is better to create a Driver

- Drivers focus on interface for effects
- It doesn't contain business logic
- It receive inputs and return outputs in a predictable way. There can be read only or write only but they are edge cases

Reactive programming using when you need to fetch lots of data from the back end

What developers needs to know about MVI (link)
