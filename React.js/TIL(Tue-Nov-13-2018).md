# TIL

# Tue Nov 13 2018

# 1. Today I Learned

# Docs



# Introducing JSX

- a syntax extension to JavaScript

- recommended that using JSX with React to describe what the UI should look like



### Embedding Expressions in JSX

- We declare a variable called `name` and then use it inside JSX by wrapping it in curly braces

- You can put any valid [JavaScript expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) inside the curly braces in JSX. (For example, `2 + 2`, `user.firstName`, or `formatName(user)` are all valid JavaScript expressions.)

- ex:

  html

```react
<div id="root"></div>
```

​	React

```react
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez',
};

const element = <h1>Hello, {formatName(user)}!</h1>;

ReactDOM.render(element, document.getElementById('root')); // 'Hello, Harper Perez!' is rendered
```



※ The primary API for rendering into the DOM looks like this:

```React
ReactDOM.render(reactElement, domContainerNode)
```



### JSX is an Expression Too

- You can use JSX inside of `if`statements and `for` loops, assign it to variables, accept it as arguments, and return it from functions.



### Specifying Attributes with JSX

- You may use quotes to specify string literals as attributes:

```React
const element = <div tabIndex="0"></div>; // 0 is value
```

- You may also use curly braces to embed a JavaScript expression in an attribute: (more frequent use)

```React
const element = <img src={user.avatarUrl}></img>; // value of user.avatarUrl is value
```



- **Warning:**

  Since JSX is closer to JavaScript than to HTML, React DOM uses `camelCase` property naming convention instead of HTML attribute names.

  For example, `class` becomes [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) in JSX, and `tabindex` becomes [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex).



### Specifying Children with JSX

- If a tag is empty, you may close it immediately with `/>`, like XML:

```react
const element = <img src={user.avatarUrl} />;
```

- JSX tags may contain children:



### JSX Prevents Injection Attacks

- It is safe to embed user input in JSX
- Everything is converted to a string before being rendered. This helps prevent [XSS (cross-site-scripting)](https://en.wikipedia.org/wiki/Cross-site_scripting) attacks



### JSX Represents Objects

- Babel compiles JSX down to `React.createElement()` calls.

  These two examples are identical:

```React
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```React
// Code that users receive actually
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

- `React.createElement()`performs a few checks to help you write bug-free code but essentially it creates an object like this:

```React
// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```

These objects are called “React elements”. 



# Rendering Elements

- Unlike browser DOM elements, React elements are plain objects, and are cheap to create. 

- React DOM takes care of updating the DOM to match the React elements.


## Rendering an Element into the DOM

- “root” DOM node
  - We call this a “root” DOM node because everything inside it will be managed by React DOM.
  - (Applications built with just React usually have a single root DOM node. If you are integrating React into an existing app, you may have as many isolated root DOM nodes as you like.)

- `ReactDOM.render()` 
  - To render a React element into a root DOM node, pass both to `ReactDOM.render()`:

```React
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

※ React Native (React for mobile)



## Updating the Rendered Element

- **Immutability(불변성)** 
  - React elements are immutable. 
  - Once you create an element, you can't changeits children or attributes.
  - The phrase that it's immutable means that you have to make a new value when you want to change the value
  -  An element is like a single frame in a movie: it represents the UI at a certain point in time.



※ Two ways to render screen

- setState
- call for rendering

**Note:**

In practice, most React apps only call `ReactDOM.render()` once. 



## React Only Updates What’s Necessary

- React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.


※ Motive for creating React: In our experience(Facebook Inc.), thinking about how the UI should look at any given moment rather than how to change it over time eliminates a whole class of bugs.



# Components and Props

Components let you split the UI into indepedent, reusable pieces, and think about each piece in isolation. 



## Function and Class Components

- The simplest way to define a component is to write a JavaScript function:

```React
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

- Class Components:
  - You can also use an ES6 class to define a component:

```React
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```



## Rendering a Component

Previously, we only encountered React elements that represent DOM tags:.

```React
const element = <div />;
```

However, elements can also represent user-defined components:

```React
const element = <Welcome name="Sara" />;
```

When React sees an element representing a user-defined component, it passes JSX attriutes to this component as a single object. We call this object "props".

For example, this code renders "Hello, Sara" on the page:

```react
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
	element,
    document.getElementById('root')
);
```





## Composing Components

Components can refer to other components in their output. This lets us use the same component abstraction for any level of detail. A button, a form, a dialog, a screen: in React apps, all those are commonly expressed as components.

For example, we can create an App component that renders Welcome many times: 

```React
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```



## Extracting Components

Extracting components might seem like grunt work at first, but having a palette of reusable components pays off in larger apps. A good rult of thumb is that if a part of your UI is used several times(Button, Panel, Avatar), or is complex enough on its own (App, FeedStory, Comment), it is a good candiate to be a reusable component.



## Props are Read-Only

Whether you declare a component as a function or a class, it must never modify its own props. Consider this sum function:

```React
function sum(a, b) {
  return a + b;
}
```

Such functions are called "pure" because they do not attempt to change their inputs, and always return the same result for the same inputs. 

In contrast, this function is impoure because it changes its own input:

```React
function withdraw(account, amount) {
  account.total -= amount;
}
```

※ (impure function ex: Math.random())

**All react components must act like pure functions with respect to their props.(render method also must act like pure function)**



# 2. Practice

- [Hello World in React(ticking clock) - Codepen](https://codepen.io/yoonjp/pen/eQBwrQ?editors=0010)

- [RGB challege - Codesandbox](https://codesandbox.io/s/20l1xm2j8y)

  - Order for coding in React

    1. Screen

    2. State

    3. Rendering from state

    4. State change 


# 3. Reference

- [React Hooks](https://reactjs.org/docs/hooks-intro.html) 

- [SyntheticEvent in React](https://reactjs.org/docs/events.html)
  - If you want to access the event properties in an asynchronous way, you should call `event.persist()`on the event, which will remove the synthetic event from the pool and allow references to the event to be retained by user code.(**it is dangerous that event listener registers in an asynchoronous way.**)

