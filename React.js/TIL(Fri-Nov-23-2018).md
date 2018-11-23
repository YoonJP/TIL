# TIL

# Fri Nov 23 2018

# 1. Today I Learned

# React

# ADVANCED GUIDES



# Higher-Order Components

A higher-order component (HOC) is an advanced technique in React for reusing component logic. HOCs are not part of the React API, per se. They are a pattern that emerges from React’s compositional nature.

Concretely, **a higher-order component is a function that takes a component and returns a new component.**

-  higher-order component is a function that creates a component, not a component. (※ If the function is a component, it must return an element.)
- A higher-order component is a function that takes a component as an argument and returns a component.

```js
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```



## Use HOCs For Cross-Cutting Concerns

- Commonly used across multiple pages (e.g. login function)
- mixins were used before HOC

- You can imagine that in a large app, this same pattern of subscribing to `DataSource` and calling `setState` will occur over and over again. We want an abstraction that allows us to define this logic in a single place and share it across many components. This is where higher-order components excel.

- Note that a HOC doesn’t modify the input component, nor does it use inheritance to copy its behavior. Rather, a HOC *composes* the original component by *wrapping* it in a container component. A HOC is a pure function with zero side-effects.



## Don’t Mutate the Original Component. Use Composition.

Resist the temptation to modify a component’s prototype (or otherwise mutate it) inside a HOC.



## Convention: Pass Unrelated Props Through to the Wrapped Componen

HOCs add features to a component. They shouldn’t drastically alter its contract. It’s expected that the component returned from a HOC has a similar interface to the wrapped component.

HOCs should pass through props that are unrelated to its specific concern. 

※ about way to use HOC (functional programming) well 



## Convention: Maximizing Composability

※ about way to use HOC (functional programming) well 



## Convention: Wrap the Display Name for Easy Debugging

The container components created by HOCs show up in the [React Developer Tools](https://github.com/facebook/react-devtools) like any other component. To ease debugging, choose a display name that communicates that it’s the result of a HOC.

The most common technique is to wrap the display name of the wrapped component. So if your higher-order component is named `withSubscription`, and the wrapped component’s display name is `CommentList`, use the display name `WithSubscription(CommentList)`:

```js
function withSubscription(WrappedComponent) {
  class WithSubscription extends React.Component {/* ... */}
  WithSubscription.displayName = `WithSubscription(${getDisplayName(WrappedComponent)})`;
  return WithSubscription;
}

function getDisplayName(WrappedComponent) {
  return WrappedComponent.displayName || WrappedComponent.name || 'Component';
}
```



### Don’t Use HOCs Inside the render Method

React’s diffing algorithm (called reconciliation) uses component identity to determine whether it should update the existing subtree or throw it away and mount a new one. If the component returned from `render` is identical (`===`) to the component from the previous render, React recursively updates the subtree by diffing it with the new one. If they’re not equal, the previous subtree is unmounted completely.

- **HOC should be applied only once**
- If the component type is changed or the key value is changed, the React will clear the state, and the HOC should not be used in the render method because the render method will be executed each time to render the component.
- Custom: Usually wrap the HOC when exporting.



### Refs Aren’t Passed Through

While the convention for higher-order components is to pass through all props to the wrapped component, this does not work for refs. That’s because `ref` is not really a prop — like `key`, it’s handled specially by React.

If you add a ref to an element whose component is the result of a HOC, the ref refers to an instance of the outermost container component, not the wrapped component.

- You can get the value of ref by another name (e.g. innerRef) and pass it through props.

※ key, ref is not passed through props



------

# React Router

- [React Router - Official Document](https://reacttraining.com/react-router/web/example/basic)

- Library that handles the state of addresses
- React Router 1 to 4 version (Each version has a different usage.)
- Library that provides the components

```js
    <Router>
      <div>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/about">About</Link>
          </li>
          <li>
            <Link to="/topics">Topics</Link>
          </li>
        </ul>

        <hr />

        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
        <Route path="/topics" component={Topics} />
      </div>
    </Router>
```

- When the address corresponding to the path of the route comes, the page corresponding to the value of the component is drawn.

  - Route acts like an if statement
  - If you do not specify a path on Route, the component is drawn on all pages.


# A brief description of the components used in the React Router

- [React-router component summary description](https://gist.github.com/seungha-kim/2810b1f14458211dfc2bcc6b061a70af )

**※ tip**

- modal pages sometimes need to be modal (e.g. posts modal on the Trello website)
-  Post id of data is good for address

## `<BrowserRouter />`, `<HashRouter />`

 

- It acts similar to the provider of the Context API.
  - In the case of the BrowserRouter, it is associated with the browser's popstate event.
  - In the case of the HashRouter, it is associated with the browser's hashchange event.
- All of the components below play a similar role as the Consumer component of the Context API. (That is, it can work with the parent Router component to get status or change state.)

## `<Link />`

- **Component to be rendered with `a` tag.** Changes the state of the address bar when clicked, and causes a page switch.
- You can specify to which address to go through `to` prop which acts as `href`.
- If the BrowserRouter is used in the parent, the address is changed via `history.pushState`, and if the HashRouter is used, the `location.hash` is changed.

**※ tip**

- The reason that `<a>` is drawn is because the browser built-in UI supports opening in a new window only by using `<a>`

## `<Route />`

- The `Route` component is a key component of the react-router and is used for selective rendering by address.
- It is only rendered when `path` prop and address match.
- `component`, `render`, `children` You can specify what to render through the prop.
  - In component prop, you can pass in the component you want to render. At this point, the component given here receives several props related to routing. (`match`, `location`, `history`)
  - The `render` prop can contain a function that returns an element. At this time, the parameters are given various information related to the routing. (`match`, `location`,` history`)
  - The use of `children` prop is similar to `render`, except that the elements returned in the functions we put here are necessarily rendered. It is used when implementing animations, etc., and is usually not used well.

**※ tip**

- You can use `render` to create an element and return it.

## `<Redirect />`

- The component whose address changes when rendered. It is used to change the address with the `Link` component.
- The `Link` component is different in that the address is changed only when the user clicks the link, while the `Redirect` component changes the address **at the moment of mounting.**
- `from` prop and `to` prop.
  - If `from` prop is omitted, it will be moved to the `to` address as soon as it is mounted.
  - If `from` prop is entered, it will be moved to the `to` address only if the current address matches from.

## `<Switch />`

- When a child node has a Route or Redirect component, Route that matches address **first** or a component that makes **only one** Redirect work.
- Here 'address match' means that the address in the browser address bar matches the `path` prop of the Route component and `from` prop of the Redirect component.

**※ tip**

- If enclosed in a switch component, it acts like 'if else' statement

- Page Route like 'NotFound' must be at the bottom of Switch. If it is above it, it will render the NotFound, and it will not render the next one.
- The specific page must be above the page that covers it. (※ The same is true for 'if else' statements.)



------

# Manipulating the browser history

[Manipulating the browser history - MDN](https://developer.mozilla.org/ko/docs/Web/API/History_API)

The DOM [`window`](https://developer.mozilla.org/en-US/docs/Web/API/Window) object provides access to the browser's history through the [`history`](https://developer.mozilla.org/en-US/docs/Web/API/Window/history) object. It exposes useful methods and properties that let you move back and forth through the user's history, as well as -- starting with HTML5 -- manipulate the contents of the history stack.

- **`back()`Moving backward**

  To move backward through history, just do:

  ```js
  window.history.back();
  ```

  This will act exactly like the user clicked on the Back button in their browser toolbar.

- **`forward()`Moving forward**

  Similarly, you can move forward (as if the user clicked the Forward button), like this:

  ```js
  window.history.forward()
  ```

- `go()`

  To move back one page (the equivalent of calling `back()`):

  ```js
  window.history.go(-1);
  ```

  To move forward a page, just like calling `forward()`:

  ```js
  window.history.go(1);
  ```

  Similarly, you can move forward 2 pages by passing 2, and so forth.

※ SPA (Single Page App) VS MPA (Mulitple Page App)

- [Single Page Applications vs Multiple Page Applications ](https://medium.com/@goldybenedict/single-page-applications-vs-multiple-page-applications-do-you-really-need-an-spa-cf60825232a3)


## Adding and modifying history entries

-  `history.pushState()` 

  ### [Example of pushState() method](https://developer.mozilla.org/en-US/docs/Web/API/History_API#Example_of_pushState()_method)

```js
var stateObj = { foo: "bar" };
history.pushState(stateObj, "page 2", "bar.html");
```

This will cause the URL bar to display http://mozilla.org/bar.html, but won't cause the browser to load `bar.html` or even check that `bar.html` exists.

`pushState()` takes three parameters: a **state** object, a **title** (which is currently ignored), and (optionally) a **URL**.



- `history.replaceState()` 

`history.replaceState()` operates exactly like `history.pushState()` except that `replaceState()` modifies the current history entry instead of creating a new one. Note that this doesn't prevent the creation of a new entry in the global browser history.



- When implementing forward and backward functions, two stacks are placed. This mechanism is also used for undo / redo implementations.


### hashChange

- Browser built-in function
- Function to create a table of contents
- Previously, using a hash to create a page switching function



**※ tip**

- location.pathname

: Get the current page address



- location.search

: Get queryString



- When you want to parse queryString

```js
const u = new URLSeachParams(location.search)

u.get('line') // "L16"

u.get('a') // "1"

u.get('b') // "2"

```



# 2. Practice

- [pushState - Codepen](https://codepen.io/yoonjp/pen/EOERmX)
- [Bulletin Board (React Router DOM) - Codesandbox]()



# 3. Reference

- [React Lifecycle Methods diagram](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)
- [Manipulating the browser history - MDN](https://developer.mozilla.org/ko/docs/Web/API/History_API)
- [Code example that created the PageContext by extracting the page switching function from the App component (in page-context branch) - Github](https://github.com/fds11/fds-react-bbs/tree/page-context/src/contexts)

- [Single Page Applications vs Multiple Page Applications ](https://medium.com/@goldybenedict/single-page-applications-vs-multiple-page-applications-do-you-really-need-an-spa-cf60825232a3)

- [page-context-pushstate - Github](https://github.com/fds11/fds-react-bbs/blob/page-context-pushstate/src/contexts/PageContext.js)
- [hashChange Example - Codepen](https://codepen.io/yoonjp/pen/yQKEWM)
- [React Router](https://reacttraining.com/react-router/web/example/basic)

- [React Router Component Summary Description](https://gist.github.com/seungha-kim/2810b1f14458211dfc2bcc6b061a70af )
