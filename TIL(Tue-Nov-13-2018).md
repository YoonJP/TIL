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



※ React가 만들어진 동기: 우리(Facebook)의 경험상, ‘시간 경과에 따라 UI를 어떻게 변경할지’를 생각하는 것이 아니라 ‘특정 순간에 UI가 어떻게 보여져야 할지’에 대해 생각하면, 수많은 종류의 버그를 없앨 수 있습니다.



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

위와 같은 함수는 입력을 변경하려 하지 않고, 동일한 입력에 대해 항상 동일한 결과를 반환하기 때문에 **[“순수”](https://en.wikipedia.org/wiki/Pure_function) 함수**라고 불립니다.

Such functions are called "pure" because they do not attempt to change their inputs, and always return the same result for the same inputs. 

In contrast, this function is impoure because it changes its own input:

```React
function withdraw(account, amount) {
  account.total -= amount;
}
```

※ (impure function ex: Math.random())

**All react components must act like pure functions with respect to their props.(render method also must act like pure function)**



# State and Lifecycle

- [**`Clock` component encapsulated - in CodePen**](http://codepen.io/gaearon/pen/dpdoYR?editors=0010)

Consider the ticking clock example from [one of the previous sections](https://reactjs.org/docs/rendering-elements.html#updating-the-rendered-element). In [Rendering Elements](https://reactjs.org/docs/rendering-elements.html#rendering-an-element-into-the-dom), we have only learned one way to update the UI. We call `ReactDOM.render()` to change the rendered output:

```react
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```





## Adding Lifecycle Methods to a Class

In applications with many components, it’s very important to free up resources taken by the components when they are destroyed.

- mounting: We want to [set up a timer](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) whenever the `Clock` is rendered to the DOM for the first time. This is called “mounting” in React.

- unmounting: We also want to [clear that timer](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/clearInterval) whenever the DOM produced by the `Clock` is removed. This is called “unmounting” in React.

```React
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  } // implement right after mounting

  componentWillUnmount() {

  } // implement right before unmounting

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

These methods are called “lifecycle methods”.

The `componentDidMount()` method runs after the component output has been rendered to the DOM. This is a good place to set up a timer:

```React
 componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```



We will tear down the timer in the `componentWillUnmount()`lifecycle method:

```React
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
```

Finally, we will implement a method called `tick()` that the `Clock` component will run every second.

※ components have lifecycle hook



## Using State Correctly

- There are three things you should know about `setState()`.


### Do Not Modify State Directly

For example, this will not re-render a component:

```React
// Wrong
this.state.comment = 'Hello';
```

Instead, use `setState()`:

```React
// Correct
this.setState({comment: 'Hello'});
```

The only place where you can assign `this.state` is the constructor.



### State Updates May Be Asynchronous

React may batch multiple `setState()` calls into a single update for performance.

Because `this.props` and `this.state` may be updated asynchronously, **you should not rely on their values for calculating the next state.** 

For example, this code may fail to update the counter:

```React
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

To fix it, use a second form of **`setState()` that accepts a function rather than an object**. That function will receive the previous state as the first argument, and the props at the time the update is applied as the second argument:

```React
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

We used an [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) above, but it also works with regular functions:

```React
// Correct
this.setState(function(state, props) {
  return {
    counter: state.counter + props.increment
  };
});
```



### State Updates are Merged

When you call `setState()`, React merges the object you provide into the current state.(fuctions like `Object.assign()`)



Then you can update them independently with separate `setState()` calls:

```React
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

The merging is shallow, so `this.setState({comments})`leaves `this.state.posts` intact, but completely replaces `this.state.comments`.

※ When duplicated objects in object, problem could occurs. 

→ So if you want to use an object having objects, you should consider shallow merging first.



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

