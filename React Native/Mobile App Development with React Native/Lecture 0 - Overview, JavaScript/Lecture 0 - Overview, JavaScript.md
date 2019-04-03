# Mobile App Development with React Native

---

# Lecture 0: Overview, JavaScript

## JavaScript is Interpreted

- Each browser has its own JavaScript engine, which either interprets the code, or uses some sort of lazy compilation
  - V8: Chrome and Node.js
  - SpiderMonkey: Firefox
  - JavaScriptCore: Safari
  - Chakra: Microsoft Edge/IE
- They each implement the ECMAScript standard, but may differ for anything not defined by the standard



## Syntax

```js
const firstName = "jordan";
const lastName = 'Hayashi';
const arr = ['teaching', 42, true, function() { console.log('hi') }];

// hi I'm a comment
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
```



## Types

- Dynamics typing
- Primitive types (no methods, immutable)
  - undefined
  - null
  - boolean
  - number
  - string
  - (symbol)
- Objects



## Typecasting? Coercion

- Explicit vs. Implicit coercion
  - const x = 42;
  - const explicit = String(x);    // explicit === "42"
  - const implicit = x + "";    // implicit === "42"

- == vs. ===

  - == coerces the types
  - === requires equivalent types

- Example

  ```js
  const x = 42
  
  // get type by using "typeof"
  console.log(typeof x)    // return "number"
  console.log(typeof undefined)    // return "undefined"
  
  // this may surprise you...
  console.log(typeof null)    // return "object"
  ```

- JavaScript Equality Table

![JavaScript-Equality-Table](/Users/yoonjaepark/TIL/React Native/Mobile App Development with React Native/Lecture 0 - Overview, JavaScript/img/JavaScript-Equality-Table.png)

https://github.com/dorey/JavaScript-Equality-Table



## Coercion, cont.

- Which values are falsy?
  - undefined
  - null
  - false
  - +0, -0, NaN
  - ""
- Which values are truthy?
  - {}
  - []
  - Everything else



## Objects, Arrays, Functions, Objects

- ^ did I put Objects twice?
- Nope, I put it 4 times.



- Everything else is an object
- Prototypal Inheritance (more on this later)



## Primitive vs. Objects

- Primitives are immutable
- Objects are mutable and store by reference



- Passing by reference vs. passing by value

- Example 

  - Ojbects

    ```js
    const o = new Object()
    o.firstName = 'Jordan'
    o.lastName = 'Hayashi'
    o.isTeaching = true
    o.greet = function() { console.log('Hello!') }
    
    console.log(JSON.stringify(o))
    
    const o2 = {}
    o2['firstName'] = 'Jordan'
    const a = 'lastName'
    o2[a] = 'Hayashi'
    
    const o3 = {
      firstName: 'Jordan',
      lastName: 'Hayashi',
      greet: function() {
        console.log('hi')
      },
      address: {
        street: "Main st.",
        number: '111'
      }
    }
    ```

  - Object mutation

    ```js
    const o = {
      a: 'a',
      b: 'b',
      obj: {
        key: 'key',
      },
    }
    
    const o2 = o
    
    o2.a = 'new value'
    
    // o and o2 reference the same object
    console.log(o.a)
    
    // this shallow-copies o into o3
    const o3 = Object.assign({}, o)
    
    // deep copy
    function deepCopy(obj) {
      // check if vals are objects
      // if so, copy that object (deep copy)
      // else return the value
      const keys = Object.keys(obj)
    
      const newObject = {}
    
      for (let i = 0; i < keys.length; i++) {
        const key = keys[i]
        if (typeof obj[key] === 'object') {
          newObject[key] = deepCopy(obj[key])
        } else {
          newObject[key] = obj[key]
        }
      }
    
      return newObject
    }
    
    const o4 = deepCopy(o)
    
    o.obj.key = 'new key!'
    console.log(o4.obj.key)
    
    ```




## Prototypal Inheritance

- Non-primitive types have a few properties/methods associated with them
  - Array.prototype.push()
  - String.prototype.toUpperCase()
- Each object stores a reference to its prototype
- Properties/methods defined most tightly to the instance have priority



## Prototypal Inheritance

- Most primitive types have object wrappers
  - String()
  - Number()
  - Boolean()
  - Object()
  - (Symbol())



## Prototypal Inheritance

- JS will automatically "box" (wrap) primitive values so you have access to methods

```js
42.toString()				 // Errors
const x = 42;
x.toString()				 // "42
x.__proto__					 // [Number: 0]
x instanceof Number  // false
```



## Prototypal Inheritance

- Why use reference to prototype?
- What's the alternative?
- What's the danger?



## Scope

- Variable lifetime

  - Lexical scoping (var): from when they're declared until when their function ends

  - Block scoping (const, let): until the next } is reached

  - Example

    ```js
    // "var" is lexically scoped, meaning it exists from time of declaration to end of func
    if (true) {
      var lexicallyScoped = 'This exists until the end of the function'
    }
    
    console.log(lexicallyScoped)
    
    // "let" and "const" are block scoped
    if (true) {
      let blockScoped = 'This exists until the next }'
      const alsoBlockScoped = 'As does this'
    }
    
    // this variable doesn't exist
    console.log(typeof blockScoped)
    
    thisIsAlsoAVariable = "hello"
    
    const thisIsAConst = 50
    
    // thisIsAConst++ // error!
    
    const constObj = {}
    
    // consts are still mutable
    constObj.a = 'a'
    
    let thisIsALet = 51
    thisIsALet = 50
    
    // let thisIsALet = 51 // errors!
    
    var thisIsAVar = 50
    thisIsAVar = 51
    var thisIsAVar = 'new value!'
    ```

- Hoisting 

  - Function definitions are hoisted, but not lexically-scoped initializations

  - Example

    ```js
    // functions are hoisted
    hoistedFunction()
    
    // but only if they are declared as functions and not as variables initialized to
    // anonymous functions
    console.log("typeof butNotThis: " + typeof butNotThis)
    
    function thisShouldWork() {
        console.log("functions are hoisted")
    }
    
    var butNotThis = function() {
        console.log("but variables aren't")
    }
    ```

- But how/why?



## The JavaScript Engine

- Before executing the code, the engine reads the entire file and will throw a syntax error if one is found
  - Any function definitions will be saved in memory
  - Variable initializations will not be run, but lexically-scoped variable names will be declared 



## The Global Object

- All variables and functions are actually parameters and methods on the global object
  - Browser global object is the `window` object
  - Node.js global object is the `global` object



## Closures

- Functions that refer to variables declared by parent function
- Possible because of scoping