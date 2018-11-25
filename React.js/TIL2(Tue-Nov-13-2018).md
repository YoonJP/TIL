# TIL

# Tue Nov 13 2018

# 1. Today I Learned

# Docs



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



# Handling Events

Handling events with React elements is very similar to handling events on DOM elements. There are some syntactic differences:

For example, the HTML:

```react
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

is slightly different in React:

```react
<button onClick={activateLasers}>
  Activate Lasers
</button>
```



- `addEventListner()`
  - If you want to access the event properties in an asynchronous way, you should call `event.persist()`on the event, which will remove the synthetic event from the pool and allow references to the event to be retained by user code.(**it is dangerous that event listener registers in an asynchoronous way.** )



```react
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    // this.handleClick = this.handleClick.bind(this);
    
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    // when using event listener, use an arrow function because this in an arrow fuction is fixed
    return (
      <button onClick={() => this.handleClick()}> 
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

You have to be careful about the meaning of `this` in JSX callbacks. In JavaScript, class methods are not [bound](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) by default. (this means that this is not fixed.) If you forget to bind `this.handleClick` and pass it to `onClick`, `this` will be `undefined` when the function is actually called.

If you aren’t using class fields syntax, you can use an [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) in the callback:

```react
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```



# Conditional Rendering

**(The contents in this chapter will be used frequently)**

In React, you can create distinct components that encapsulate behavior you need. Then, you can render only some of them, depending on the state of your application.

We’ll create a `Greeting` component that displays either of these components depending on whether a user is logged in:

```react
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```



### Inline If with Logical && Operator

You may [embed any expressions in JSX](https://reactjs.org/docs/introducing-jsx.html#embedding-expressions-in-jsx) by wrapping them in curly braces. This includes the JavaScript logical `&&` operator. It can be handy for conditionally including an element:

```react
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

※ when rendering true, false, null in JS, nothing is rendered.



### Inline If-Else with Conditional Operator

Another method for conditionally rendering elements inline is to use the JavaScript conditional operator [`condition ? true : false`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator).

In the example below, we use it to conditionally render a small block of text.

```react
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```



### Preventing Component from  Rendering

In rare cases you might want a component to hide itself even though it was rendered by another component. To do this return `null` instead of its render output.

In the example below, the `<WarningBanner />` is rendered depending on the value of the prop called `warn`. If the value of the prop is `false`, then the component does not render:

```react
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true};
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(state => ({
      showWarning: !state.showWarning
    }));
  }

  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}

ReactDOM.render(
  <Page />,
  document.getElementById('root')
);
```

[**Try it on CodePen**](https://codepen.io/gaearon/pen/Xjoqwm?editors=0010)

Returning `null` from a component’s `render` method does not affect the firing of the component’s lifecycle methods. For instance `componentDidUpdate` will still be called.



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
