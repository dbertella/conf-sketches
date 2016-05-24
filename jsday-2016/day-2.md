# Day 2

## Keynote: Shipping one of the largest Microsoft JavaScript applications (Visual Studio Code's story)
Alexandru Dima is a Microsoft developer in the VS Code team and he did a very great talk about the story of their product.
It was really helpful to understand at least why Microsoft didn't just fork *Atom editor* but create another one with same technology.
Basically they started in 2014 working on it using `node-webkit` as a platform switching to `electron` later on.
He talked about the roadmap they had, early preview in April, beta in November 2015 and 1.0 in April 2016.
Nice insights on the product, sure something to play with when you get bored of *Atom*.

## Building Reactive Architectures
The talk by @mattpodwysocki was great, he took the first day workshop's presentation and explain it further, again *Functional reactive programming*, but a really nice and short talk, with all you need to know about RxJs.
This man is a great on the stage with his sorcerer hat in the head. Nice talk to follow in my opinion.

## Discover the information within your data with d3.js
Not much to say about it, it was a really entry level talk by Daniela Mogini. Some insights on how to manipulate DOM, handling Array of data natively in D3js. The entry point was this is not jQuery but seems a lot like it to me in general. I know that D3.js it's really powerful but at the end of this talk I haven't feel much of the awesomeness.

## The Evolution of Asynchronous JavaScript
@cirpo gave us another really good talk about async in javascript. He started with history, how cool callbacks are and how, if you aren't a lazy developer, you can avoid the feared callback hell.

He Talked about Promises, of course, and how they become the official way to provide async in js.
But Promises aren't always the best choice, don't use them in flow control. You can end up easily with a **Promise Hell** if you don't pay attention.

> **Generators + Promises** for the win.

*Generators*, a new type of function that doesn't have the *run-to-completion* behaviour of the other function.
They have a *yield* keyword that can pause/restart the function! You have full control on it then.
You have to use libraries to use them but seems like with the next javascript ES7 (ehm ES2016) will ship async / await natively.
You can start playing with it using babel (or typescript ndr).
```
npm install -g babel-cli
npm install babel-plugin-transform-async-to-generator

//add it either to .babelrc or package.json
{
  "plugins": ["transform-async-to-generator"]
}

babel a.es6 -o a.js
node a.js
```

**Conclusion**
 - callbacks
 - promises
 - async/await
 - highland (RxJs, streams)

> CHOOSE YOUR CONCURRENCY MODEL

## (Web?) Components in production
@olamad313 is a UX Designer at Amazon Web Services and gave us a great view about how they handle components in their teams across the world. She didn't speak about libraries or polyfill (*Polimer* for example), they don't actually use one apparently because they didn't want to depend on it.

They build their component using plain javascript embracing web components philosophy and then they also build the bridge to connect components with frameworks or libraries teams choose to use (GWT, Angular, and React).

They keep principles like custom elements and style encapsulation with few benefits: easy to grab for new teams and no longer need to review the components final implementation. Little effort after a redesign and sparked community engagement.

## FFTT: A new concept of build tool
@m_a_s_s_i talks about his side project, and invite every one (who can really understand what is it about) to contribute on it. Basically he's building his own build tool to replace `grunt`, `gulp` or `webpack` (I'm writing only the most famous one).
Unsatisfied with the job those kind of tool is doing @m_a_s_s_i writes his own to accomplish what he really need: a fast, deterministic, reliable and language agnostic build tool.

The build system meet functional programming, immutable data structures, memoization of functions result, each step is a pure function.
You never mess with the environment, it's just data transformation other data.
The build graph is declarative and functional. Each build step is imperative but inside a *container*, so you don't mess up with system.

It's built with not many lines of javascript but it's totally programming language independent.
It's not ready for production yet but feel free to contribute if you are interested in it! Here the github [link](https://github.com/massimiliano-mantione/fftt)

## Higher Order Components in React
Finally React :-D. Thanks to @cef62.
After an excursus on React in general, and the component concept in particular we learn how to extend a component using an high order one. He spoke about mixins (deprecated with ES6 syntax) as well and he gave us a good `HoC vs Mixins` preview:
- Declarative vs Imperative
- Customizable vs Favors inheritance
- Easy to read vs Hard to read
- Enforce encapsulation

He explains *high order function* as functions that accept a function as an argument and return another function.
So HOC are high order functions that, instead, return a component.

## A Class Action
Last talk of the conference by @unlucio, a dispute on probably the most controversial feature in ES2016: the `class` keyword.

He called few witnesses for the *dispute*:
  - `miss function`
  > do we really need a shorthand for manually defining a constructor Function?

  - `the prototype`
  - `class`
  > Does she really add something new to the language and will she simplify everybody's life

*OOP is good for you... at least that what they say*
![OOP is good for you... at least that what they say](http://thinknsmile.com/wp-content/uploads/2014/05/butter_is_good_for_you.jpg)

If you want to write once and use your code in multiple situation, prototypal inheritance makes this easy not classes.

For code segregation choose `modules` over `classes`

When coffee is good you don't need sugar, but `class` seems more like aspartame than sugar.
The difference is that aspartame can kill you!

To sum up it was a really nice conclusion for a really nice two days conference in Verona.
I encourage you to have a look at [slides](http://www.slideshare.net/unlucio/a-class-action) that are self explanatory.
