# TIL

# Wed Nov 14 2018



# 1. Today I Learned

# React



# Lists and Keys

This chapter is about way to handle UI as value



### Basic List Component

- assign a `key` to our list items inside `numbers.map()`



## Keys

Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity:

```js
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```

- The best way to pick a key is to use a string that uniquely identifies a list item among its siblings. Most often you would use IDs from your data as keys:

```js
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

- When you don’t have stable IDs for rendered items, you may use the item index as a key as a last resort:

```js
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
);
```

- We don’t recommend using indexes for keys if the order of items may change. (When you use item index of array, you should be careful to use it.) 

※ All keys should be different values each other because key is used to identify item.



### Extracting Components with Keys



### Keys Must Only Be Unique Among Siblings

- Keys used within arrays should be unique among their siblings. However they don’t need to be globally unique. We can use the same keys when we produce two different arrays:

```js
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key={post.id}> 
          {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
);
```



※ key, ref can't be used in component 



※ **When class is rendered, class generates instance of the class and the instance is called  `this`. And when screen is rendered again, the instance that remembers state is deleted.**  



※ You can change value of key to reset state because when key change, new state is generated again.



# Forms

HTML form elements work a little bit differently from other DOM elements in React, because form elements naturally keep some internal state. 

 In most cases, it’s convenient to have a JavaScript function that handles the submission of the form and has access to the data that the user entered into the form. The standard way to achieve this is with a technique called “**controlled components**”. (**↔ uncontrolled components**)



## Controlled Components

In HTML, form elements such as `<input>`, `<textarea>`, and `<select>` typically maintain their own state and update it based on user input. In React, mutable state is typically kept in the state property of components, and only updated with [`setState()`](https://reactjs.org/docs/react-component.html#setstate).

We can combine the two by making the React state be the “**single source of truth**”. Then the React component that renders a form also controls what happens in that form on subsequent user input. An input form element whose value is controlled by React in this way is called a “**controlled component**”.



Since the `value` attribute is set on our form element, the displayed value will always be `this.state.value`, making the React state the source of truth. Since `handleChange`runs on every keystroke to update the React state, the displayed value will update as the user types.



※ In DOM, you should use input event, you should use onChange in React.



### ※ controlled component makes `<textarea>`, `<select>`, `<input>` just function to render the screen.



## Alternatives to Controlled Components

It is necessary to be able to bring DOM object to React in order to use uncontrolled component well. (querySelector can't bring DOM object to React.)



# Lifting State Up

State needs to be lifted up when many children need shared state. 



# Composition vs Inheritance

React has a powerful composition model, and we recommend using composition instead of inheritance to reuse code between components.



## Containment

Some components don’t know their children ahead of time. This is especially common for components like `Sidebar` or `Dialog` that represent generic “boxes”.

We recommend that such components use the special `children` prop to pass children elements directly into their output.



## Specialization

Sometimes we think about components as being “special cases” of other components. For example, we might say that a `WelcomeDialog` is a special case of `Dialog`.

In React, this is also achieved by composition, where a more “specific” component renders a more “generic” one and configures it with props.



# 2. Practice

- [[Controlled Text Example] - Codepen](https://codepen.io/gaearon/pen/VmmPgp?editors=0010)
- [[Controlled Select Example] - Codepen](https://codepen.io/yoonjp/pen/GwWobx?editors=0010)
- [props.children - Codepen](https://codepen.io/yoonjp/pen/oQZxWo?editors=0010)

- [To-do List - Codesandbox](https://codesandbox.io/s/x3jkx1y4pp)


# 3. Reference

## JS

- [Modules before ES2015](https://d2.naver.com/helloworld/12864)


## React

- [in-depth explanation about why keys are necessary](https://reactjs.org/docs/reconciliation.html#recursing-on-children) 
- [Formik: controlled component library](https://github.com/jaredpalmer/formik)

- [classNames Library - npm](https://www.npmjs.com/package/classnames)
