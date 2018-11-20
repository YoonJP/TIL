# TIL

# Tue Nov 20 2018

# 1. Today I Learned

# Uncontrolled Components

In most cases, it is recommended to use [controlled components](https://reactjs.org/docs/forms.html) to implement forms. In a controlled component, form data is handled by a React component

- To write an uncontrolled component, instead of writing an event handler for every state update, you can [use a ref](https://reactjs.org/docs/refs-and-the-dom.html) to get form values from the DOM.
- Since an uncontrolled component keeps the source of truth in the DOM, it is sometimes easier to integrate React and non-React code when using uncontrolled components. 



### Default Values

With an uncontrolled component, you often want React to specify the initial value, but leave subsequent updates uncontrolled. To handle this case, you can specify a `defaultValue` attribute instead of `value`.

```js
<input defaultValue="Bob" type="text" ref={this.input} />
```

Likewise, `<input type="checkbox">` and `<input type="radio">` support `defaultChecked`, and `<select>` and `<textarea>` supports `defaultValue`.



## The file input Tag

In React, an `<input type="file" />` is always an uncontrolled component because its value can only be set by a user, and not programmatically. 



# Optimizing Performance

(pretty important issue in React)

Internally, React uses several clever techniques to minimize the number of costly DOM operations required to update the UI. For many applications, using React will lead to a fast user interface without doing much work to specifically optimize for performance. Nevertheless, there are several ways you can speed up your React application.

(If `setState()` in component is implemented (or if state is changed), performance could be lowered because  every elements are checked by calling all  `render()` methods.)



### `React.PureComponent`

`React.PureComponent` is similar to [`React.Component`](https://reactjs.org/docs/react-api.html#reactcomponent). The difference between them is that [`React.Component`](https://reactjs.org/docs/react-api.html#reactcomponent) doesn’t implement [`shouldComponentUpdate()`](https://reactjs.org/docs/react-component.html#shouldcomponentupdate), but `React.PureComponent`implements it with a shallow prop and state comparison.

If your React component’s `render()` function renders the same result given the same props and state, you can use `React.PureComponent` for a performance boost in some cases.



## Avoid Reconciliation

`shouldComponentUpdate` lifecycle hook method is in PureComponent

```js
shouldComponentUpdate(nextProps, nextState) {
  return true;
}
```

If you know that in some situations your component doesn’t need to update, you can return `false` from `shouldComponentUpdate` instead, to skip the whole rendering process, including calling `render()` on this component and below.



## shouldComponentUpdate In Action

![shouldComponentUpdate](https://reactjs.org/static/should-component-update-5ee1bdf4779af06072a17b7a0654f6db-9a3ff.png)



# Reconciliation

React provides a declarative API so that you don’t have to worry about exactly what changes on every update. This makes writing applications a lot easier, but it might not be obvious how this is implemented within React.



## Motivation

React implements a heuristic O(n) algorithm based on two assumptions:

1. Two elements of different types will produce different trees.
2. The developer can hint at which child elements may be stable across different renders with a `key` prop.



## The Diffing Algorithm



### Elements Of Different Types

When tearing down a tree, old DOM nodes are destroyed. Component instances receive `componentWillUnmount()`. When building up a new tree, new DOM nodes are inserted into the DOM. Component instances receive `componentWillMount()` and then `componentDidMount()`. Any state associated with the old tree is lost.

Any components below the root will also get unmounted and have their state destroyed.



### DOM Elements Of The Same Type

When comparing two React DOM elements of the same type, React looks at the attributes of both, keeps the same underlying DOM node, and only updates the changed attributes.

```js
// By comparing these two elements, React knows to only modify the className on the underlying DOM node.

<div className="before" title="stuff" />

<div className="after" title="stuff" />
```

```js
// When updating style, React also knows to update only the properties that changed. For example:

<div style={{color: 'red', fontWeight: 'bold'}} />

<div style={{color: 'green', fontWeight: 'bold'}} />

// When converting between these two elements, React knows to only modify the color style, not the fontWeight.
```



### Component Elements Of The Same Type



When a component updates, the instance stays the same, so that state is maintained across renders. React updates the props of the underlying component instance to match the new element, and calls `componentWillReceiveProps()` and `componentWillUpdate()` on the underlying instance.

Next, the `render()` method is called and the diff algorithm recurses on the previous result and the new result.



### Keys

When children have keys, React uses the key to match children in the original tree with children in the subsequent tree.

The key only has to be unique among its siblings, not globally unique. 



- [EX: when key has been changed, state is deleted - Codesandbox](https://codesandbox.io/s/7mxww787j6)



# 2. Practice

- [Optimizing Performance - Codesandbox](https://codesandbox.io/s/x799w40wqz)
- [Immutability - Codesandbox](https://codesandbox.io/s/62o487vq1n)
- [Immutable js - List - Repl](https://repl.it/@YoonJP/Immutable-js?language=javascript)



# 3. Reference

- [formik: uncontrolled component library - Github](https://github.com/jaredpalmer/formik)
- [immutable js](https://facebook.github.io/immutable-js/)
  - List(like array)
  - Map(like object)
- [Immer library - Github](https://github.com/mweststrate/immer)
- [shouldComponentUpdate before&after application - YouTube](https://www.youtube.com/watch?v=7jY1ABmuxr8)
