# TIL

# Mon Nov 19 2018

# 1. Today I Learned

# React

# ADVANCED GUIDES

# JSX in Depth

Fundamentally, JSX just provides syntactic sugar for the `React.createElement(component, props, ...children)` function. The JSX code:

```js
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>
```

compiles into:

```js
React.createElement(
  MyButton,
  {color: 'blue', shadowSize: 2},
  'Click Me'
)
```



## Specifying The React Element Type

The first part of a JSX tag determines the type of the React element.

Capitalized types indicate that the JSX tag is referring to a React component. These tags get compiled into a direct reference to the named variable, so if you use the JSX `<Foo />` expression, `Foo` must be in scope.



### React Must Be in Scope

Since JSX compiles into calls to `React.createElement`, the `React` library must also always be in scope from your JSX code.



### Using Dot Notation for JSX Type

You can also refer to a React component using dot-notation from within JSX. 

```js
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```





### User-Defined Components Must Be Capitalized

When an element type starts with a lowercase letter, it refers to a built-in component like `<div>` or `<span>` and results in a string `'div'` or `'span'` passed to `React.createElement`. Types that start with a capital letter like `<Foo />` compile to `React.createElement(Foo)` and correspond to a component defined or imported in your JavaScript file.



### Choosing the Type at Runtime

You cannot use a general expression as the React element type. If you do want to use a general expression to indicate the type of the element, just assign it to a capitalized variable first.



### Props Default to “True”

```js
// These two JSX expressions are equivalent:

<MyTextBox autocomplete />

<MyTextBox autocomplete={true} /> 
// this form is recommended by React Official guide
```



### Spread Attributes

```js
// These two components are equivalent:

function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```

You can also pick specific props that your component will consume while passing all other props using the spread operator.

```js
const Button = props => {
  const { kind, ...other } = props;
  const className = kind === "primary" ? "PrimaryButton" : "SecondaryButton";
  return <button className={className} {...other} />;
};

const App = () => {
  return (
    <div>
      <Button kind="primary" onClick={() => console.log("clicked!")}>
        Hello World!
      </Button>
    </div>
  );
};
```

※ this is used frequently to make a custom component



## Children in JSX

### String Literals

### JSX Children

### JavaScript Expressions as Children



### Functions as Children

```js
// Calls the children callback numTimes to produce a repeated component
function Repeat(props) {
  let items = [];
  for (let i = 0; i < props.numTimes; i++) {
    items.push(props.children(i));
  }
  return <div>{items}</div>;
}

function ListOfTenThings() {
  return (
    <Repeat numTimes={10}>
      {(index) => <div key={index}>This is item {index} in the list</div>}
    </Repeat>
  );
}
```



### Booleans, Null, and Undefined Are Ignored

`false`, `null`, `undefined`, and `true` are valid children. They simply don’t render. 

This can be useful to conditionally render React elements. This JSX only renders a `<Header />` if `showHeader` is `true`:

```js
<div>
  {showHeader && <Header />}
  <Content />
</div>
```

One caveat is that some [“falsy” values](https://developer.mozilla.org/en-US/docs/Glossary/Falsy), such as the `0` number, are still rendered by React. 

```js
<div>
  {props.messages.length &&
    <MessageList messages={props.messages} />
  }
</div>

// To fix this, make sure that the expression before `&&` is always boolean:

<div>
  {props.messages.length > 0 &&
    <MessageList messages={props.messages} />
  }
</div>
```





# Static Type Checking

- Static type checkers like [Flow](https://flow.org/) and [TypeScript](https://www.typescriptlang.org/) identify certain types of problems before you even run your code. 
- They can also improve developer workflow by adding features like auto-completion. 

For this reason, it is recommended to use Flow or TypeScript instead of `PropTypes` for larger code bases.



## Static Type VS Dynamic Type

- Static typing Language
  - type checking is done at compile-time
  - possible to find bug regarding type before running code
  - ex: C, C++, Java, Swift, Kotlin...
- Dynamic typing Language
  - type checking is done at run-time
  - hard to find bug regarding type before before running code
  - ex: JavaScript, Ruby, Python...
- Language that adds type checking function at complie-time to JavaScript
  - ex: TypeScript, Flow (made by Facebook)



# Refs and the DOM

Refs provide a way to access DOM nodes or React elements created in the render method.



### When to Use Refs

There are a few good use cases for refs:

- Managing focus, text selection, or media playback.
- Triggering imperative animations.
- Integrating with third-party DOM libraries.



### Creating Refs

Refs are created using `React.createRef()` and attached to React elements via the `ref` attribute. Refs are commonly assigned to an instance property when a component is constructed so they can be referenced throughout the component. (like arrow)

```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```



### Accessing Refs

When a ref is passed to an element in `render`, a reference to the node becomes accessible at the `current` attribute of the ref.

```js
const node = this.myRef.current;
```



The value of the ref differs depending on the type of the node:

- When the `ref` attribute is used on an HTML element, the `ref` created in the constructor with `React.createRef()` receives the underlying DOM element as its `current` property.
- When the `ref` attribute is used on a custom class component, the `ref` object receives the mounted instance of the component as its `current`.
- **You may not use the ref attribute on function components** because they don’t have instances.



------



# Create React App

- [Facebook/create-react-app - Github](https://github.com/facebook/create-react-app)

Create React apps with no build configuration.

 

## Quick Overview

```terminal
npx create-react-app my-app
cd my-app
npm start
```



------



# Create React App

[User Guide](https://facebook.github.io/create-react-app/docs/folder-structure)



# Supported Browsers and Features

## Supported Browsers

By default, the generated project supports all modern browsers. Support for Internet Explorer 9, 10, and 11 requires [polyfills](https://github.com/facebook/create-react-app/blob/master/packages/react-app-polyfill/README.md).



## Supported Language Features

This project supports a superset of the latest JavaScript standard. In addition to [ES6](https://github.com/lukehoban/es6features) syntax features, it also supports:

- [Exponentiation Operator](https://github.com/rwaldron/exponentiation-operator) (ES2016).
- [Async/await](https://github.com/tc39/ecmascript-asyncawait) (ES2017).
- [Object Rest/Spread Properties](https://github.com/tc39/proposal-object-rest-spread) (ES2018).
- [Dynamic import()](https://github.com/tc39/proposal-dynamic-import) (stage 3 proposal)
- [Class Fields and Static Properties](https://github.com/tc39/proposal-class-public-fields) (part of stage 3 proposal).
- [JSX](https://facebook.github.io/react/docs/introducing-jsx.html), [Flow](https://facebook.github.io/create-react-app/docs/adding-flow) and [TypeScript](https://facebook.github.io/create-react-app/docs/adding-typescript).



# Adding a Stylesheet

This project setup uses [Webpack](https://webpack.js.org/) for handling all assets. Webpack offers a custom way of “extending” the concept of `import` beyond JavaScript. 



※ base64 - [Base64 - Wikipedia](https://en.wikipedia.org/wiki/Base64)



# 2. Practice 

- [To-do List - Codesandbox](https://codesandbox.io/s/105pvv0x13)



# 3. Reference

- [Facebook/create-react-app - Github](https://github.com/facebook/create-react-app)
- [User Guide](
