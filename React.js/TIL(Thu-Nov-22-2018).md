# TIL

# Thu Nov 22 2018



# 1. Today I Learned

# React

# Composition vs Inheritance



## Containment

Some components don’t know their children ahead of time. This is especially common for components like `Sidebar` or `Dialog` that represent generic “boxes”.

We recommend that such components use the special `children`prop to pass children elements directly into their output:

```js
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```



------

# ADVANCED GUIDES

# Context

Context provides a way to pass data through the component tree without having to pass props down manually at every level.



In a typical React application, data is passed top-down (parent to child) via props, but this can be cumbersome for certain types of props (e.g. locale preference, UI theme (like Dark theme)) that are required by many components within an application. Context provides a way to share values like these between components without having to explicitly pass a prop through every level of the tree.



※ Redux was used before Context API was released. 



```js
const ThemeContext = React.createContext('light');
```

```react
<ThemeContext.Provider value="dark">
	<Toolbar />
</ThemeContext.Provider>
```

```React
<ThemeContext.Consumer>
  {theme => <Button {...props} theme={theme} />}
</ThemeContext.Consumer>
```

※ React finds the nearest Provider and uses its value



## API

### `React.createContext`

```
const {Provider, Consumer} = React.createContext(defaultValue);
```

Creates a `{ Provider, Consumer }` pair.  When React renders context `Consumer`, it reads the current context value from the nearest top `Provider` created from the same context. 

- Provider and Consumer apply only to those created from the same context.

- The `defaultValue` argument is **only** used when a component does not have a matching Provider above it in the tree. This can be helpful for testing components in isolation without wrapping them. 



### `Provider`

```
<Provider value={/* some value */}>
```

Every Context object comes with a Provider React component that allows consuming components to subscribe to context changes.



### `Consumer`

```
<Consumer>
  {value => /* render something based on the context value */}
</Consumer>
```

A React component that subscribes to context changes. This lets you subscribe to a context within a [function component](https://reactjs.org/docs/components-and-props.html#function-and-class-components).



- All consumers that are descendants of a Provider will re-render whenever the Provider’s `value` prop changes. 
- Not rendered if `value` is the same
- Not affected by `shouldComponentUpdate`

※ context Usage example: My information used in several pages



## Examples

- You can drop the value of the provider to the state of the component in which the provider is located.
- The provider can drop an object by value.



### Updating Context from a Nested Component

**theme-toggler-button.js**

```js
import {ThemeContext} from './theme-context';

function ThemeTogglerButton() {
  // The Theme Toggler Button receives not only the theme
  // but also a toggleTheme function from the context
  return (
    <ThemeContext.Consumer>
      {({theme, toggleTheme}) => (
        <button
          onClick={toggleTheme}
          style={{backgroundColor: theme.background}}>
          Toggle Theme
        </button>
      )}
    </ThemeContext.Consumer>
  );
}

export default ThemeTogglerButton;
```



Putting a value object in the render method creates a new object each time the render method is called, so you can put the value object in state to avoid it.



## Precautions

Because the Context compares the references of the values to determine when the consumer should be re-rendered, the consumer may be rendered unnecessarily re-rendered when the provider's parent is rendered. For example, the following code re-renders all consumers when the Provider is re-rendered, because a new object is passed to the `value` every time:



```js
class App extends React.Component {
  render() {
    return (
      <Provider value={{something: 'something'}}>
        <Toolbar />
      </Provider>
    );
  }
}
```



To avoid this problem, store the object to be used as value in the parent's state:

```js
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: {something: 'something'},
    };
  }

  render() {
    return (
      <Provider value={this.state.value}>
        <Toolbar />
      </Provider>
    );
  }
}
```



# Fragments

A common pattern in React is for a component to return multiple elements. Fragments let you group a list of children without adding extra nodes to the DOM.

```react
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  );
}
```



### Short Syntax

There is a new, shorter syntax you can use for declaring fragments. It looks like empty tags:

```js
class Columns extends React.Component {
  render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
```



### Keyed Fragments

`key` is the only attribute that can be passed to `Fragment`. (In the future, we may add support for additional attributes, such as event handlers.)



# Portals

Portals provide a first-class way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.

- A typical use case for portals is when a parent component has an `overflow: hidden` or `z-index` style, but you need the child to visually “break out” of its container. For example, dialogs, hovercards, and tooltips.
- Can be drawn outside of root (ex: modal, popup(when the z-index should be high but the z-index can not be resolved)) → **stacking context [[Stacking context - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)]** 

※ TIP: Usually you will use the portal with the library.



## Event Bubbling Through Portals

Even though a portal can be anywhere in the DOM tree, it behaves like a normal React child in every other way. Features like context work exactly the same regardless of whether the child is a portal, as the portal still exists in the *React tree* regardless of position in the *DOM tree*.

- This includes event bubbling. An event fired from inside a portal will propagate to ancestors in the containing *React tree*, even if those elements are not ancestors in the *DOM tree*.
- Catching an event bubbling up from a portal in a parent component allows the development of more flexible abstractions that are not inherently reliant on portals.

※ Abstraction VS Concept

- abstract (eg steering wheel, brake, accelerator ...)

- Concept (eg engine, mission ...)

  →  React is said to have good abstraction



# 2. Practice

- [Context - Codesandbox](https://codesandbox.io/s/new)
- [MenuSelector(Context) - Codesandbox](https://codesandbox.io/s/new)
- [Portals - Codepen](https://codepen.io/yoonjp/pen/oQEmNE)



# 3. Reference

- [Stacking context - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)

