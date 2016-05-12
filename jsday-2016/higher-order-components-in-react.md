# Higher Order Components in React | Matteo Ronchi

First of all web was a document

jquery changed the web, it become dinamyc

Angularjs change the approach, directive first attemp to encapsulate a component
Separation of concern in technology

React, took the template and bring it inside the javascript.

## All modern frameworks are component oriented

Everything can be a component, an application is a tree of components
**Component**: Composable, encapsulated, reusable, easy to design

## React
2 kind of component

### Presentational component
small, reusable, *pure function* => Easy to test
Stateless functional components:
`const Label = (props) => <h1>{props.children}</h1>`

### Stateful component
Components can change internal state, state is kind of immutable, we should let react mutate the state.

### Component lifecycle
Method pre or after `render()`
Presentational component has only render method

## Reduce code boilerplate

### React mixins
Previous implementations, work on the prototype of the component, object that expose particular api, from mixin I can access at internal lifecycle of the component. Really powerful but can be scary. Lot of mixins are third party developed. It can mess with data or lifecycle of my application. You can have namespace collision.
It works only with createClass method, not ES2015 classes.

### Higher Order Component
High order function. Function that accept a function as an argument and return another function.

HOC = High order function that return a compoent.

```
const Wrapped = () => <h1>Wrapped Comp</h1>
const wrap = (Comp) => () => {
  return (
    <div><Comp /></div>
  )
}
```

You can have a lot of presentational component, you can nest HOC, trying to not have 15 of them, but still you can do it and it's easy to implement.

There is a Decorator Proposal that is not a standard at the moment, and it's a kind of annotation for js classes.
Function that accept a single argument that is a component

```
@wrap
class Wrap extends Component {}
```

### HoC vs Mixins
- Declarative vs Imperative
- Customizable vs Favors inheritance
- Easy to read vs Hard to read
- Enforce encapsulation

### Best practice
- Expose wrapped component
- Pass props to wrapped component
- Expose statics methods and props from wrapped component

Sometimes you have to access the wrapped component:
```
Wrapper.wrappedComponent = Component
return Wrapper
```

`hoist-non-react-statics` to copy all the statics methods and props from a wrapped component
