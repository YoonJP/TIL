### APPENDIX A

## Modern JavaScript Syntax

Some of the code samples in this book use modern JavaScript syntax. If you're not familiar with this syntax, don't worry—it's a pretty straightforward translation from the JavaScript you might be accustomed to.

ECMAScript 5, or ES5, is the JavaScript language specification with the broadest adoption. However, there are many compelling language features introduced in ES6, ES7, and beyond. React Native uses Babel (*https://babeljs.io/*), the JavaScript compiler, to transform our JavaScript and JSX code. One of Babel's features is its ability to compile newer-style syntax into ES5-compliant JavaScript. This enables us to use language feature from ES6 and beyond throughout our React codebase. 

### let and const

In pre-ES6 JavaScript, we use `var` to declare variable.

In ES6, there are two additional ways to declare variable: `let` and `const`. A variable declared with `const` cannot be reassigned; that is to say, the following is invalid:

```
  const count = 2;
  count = count + 1 // BAD
```

Variables declared with `let` or `var` may be reassigned. A variable declared with `let` may only be used in the same block as it is defined. 

Some of the examples in this book still use `var`, but you'll also see `let` and `const`. Don't worry about the distinctions too much.

### Importing Modules

We could use CommonJS module syntax to export our components and other JavaScript modules (Example A-1). In this system, we use `require` to import other modules, and assign a value to `module.exports` in order to make a file's contents available to other modules.

*Example A-1. Requiring and exporting modules using CommonJS syntax*

```
var OtherComponent = require('./other_component');

class MyComponent extends Component {
  ...
}
  
module.exports = MyComponent;
```

With ES6 module syntax (*http://mzl.la/21cv5QF*), we can use the `export` and `import` commands instead. Example A-2 shows the equivalent code, using ES6 module syntax.

*Example A-2. Importing and exporting modules using ES6 module syntax*

```
import otherComponent from './other_component';

class MyComponent extends Component {
  ...
}
  
export default MyComponent;
```

### Destructing

Destructing assignments (*http://mzl.la/1I6ppBl*) provide us with a convenient shorthand for extracting data from objects.

Take this EX5-complaint snippet:

```
  var myObj = {a: 1, b: 2};
  var a = myObj.a;
  var b = myObj.b;
```

We can use destructing to do this more succinctly:

```
  var {a, b} = {a:1, b: 2};
```

You'll often see this used with `import` statements. When we `import` React, we're actually receiving an object. We *could* import without using destructing, as shown in Example A-3.

*Example A-3. Importing the Component class without destructing*

```
import React from "react";
let Component = React.Component;
```

But it's much nicer to use destructing as shown in Example A-4.

*Example A-4. Using destructing to import the Component class*

```
import React, { Component } from "react";
```

### Function Shorthand

ES6's function shorthand (*http://mzl.la/1SW4AJ4*) is also convenient. In ES5-compliant JavaScript, we define functions as shown in Example A-5.

*Example A-5. Longhand function declaration*

```
render: function() {
  return <Text>Hi</Text>;
}
```

Writing out `function` over and over again can get annoying. Example A-6 shows the same function, this time applying ES6's function shorthand.

*Example A-6. Shorthand function declaration*

```
render() {
  return <Text>Hi</Text>;
}
```

### Fat-Arrow Functions

In ES5-compliant Ja*vaScript, we often need to `bind` our functions to make sure that their context (i.e., the value of `this`) is as expected (Example A-7). This is especially common when we're dealing with callbacks.*

*Example A-7. Binding functions manually with ES5-compliant JavaScript*

```
var callbackFunc = function(val) {
  console.log('Do something');
}.bind(this);
```

Fat-arrow functions (*http://mzl.la/1MN2cRj*) are automatically bound so we don't need to do that ourselves (Example A-8).

*Example A-8. Using a fat-arrow function for binding*

```
var callbackFunc = (val) => {
  console.log('Do something');
};
```

### Default Parameters

You can specify default parameters for a function, as shown in Example A-9.

*Example A-9. Using default parameters*

```
var helloWorld = (name = "Bonnie") => {
        console.log("Hello, " + name);
}

helloWorld("Zach"); // Prints "Hello, Zach"
helloWorld(); // Prints "Hello, Bonnie"
```

This syntax is convenient when you want to guarantee a sensible default value for a parameter.

### String Interpolation

In ES5-compliant JavaScript, we might build a string by using code such as that in Example A-10. 

*Example A-10. String concatenation in ES5-compliant JavaScript*

```
var API_KEY = 'abdefg';
var url = 'http://someapi.com/request&key=' + API_KEY;
```

Instead, we can use tempate strings (*http://mzl.la/21cvceS*), which support multiline strings and string interpolation. By enclosing a string in backticks, we can insert other variable values using the `${}` syntax (Example A-11).

*Example A-11. String interpolation in ES6*

```
var API_KEY = 'abcdefg';
var url = `http://someapi.com/request&key=${API_KEY}`;
```

### Working with Promises

A promise is an object representing something that will eventually happen. Instead of handcrafting your handling of success and error callbacks, promises have a consistent API for interacting with asynchronous operations.

Let's ay that you have two callbacks: one for success and one for error handling (see Example A-12).

*Example A-12. Defining two callbacks*

```
function errorCallback(result) {
  console.log("It succeede: ", result);
}

function errorCallback(error) {
  console.log("It failed: ", error);
}
```

An old-style function might expect two callbacks and call one of them based on success or failure (see Example A-13).

*Example A-13. Passing success and error callbacks in old-style JavaScript*

```
uploadToSomeAPI(successCallback, errorCallback);
```

With modern promise-based syntax, you can pass success and error callbacks as shown in Example A-14.

```
uploadToSomeAPI().then(successCallback, errorCallback);
```

These two examples look very similar, but the advantages of using promises becomes evident when you have many callbacks or asynchronous operations to execute. Let's say that you need to upload some data to an API, update a user interface, and then look for new data. 

With old-style callbacks, we can quickly end up in what is sometimes referred to as "callback hell" (Example A-15).

*Example A-15. Chaining callbacks together can get messy quickly and is also repetitive*

```
uploadToSomeAPI(
  (result) => {
    updateUserInterface(
      result,
      uiUpdateResult => {
        checkForNewData(
          upUpdateResult,
          newDataResult => {
            successCallback(newDataResult);
          },
          errorCallback
        );
      },
      errorCallback
    );
  }, errorCallback
);
```

With promises, we can chain calls to the `then` method, as Example A-16 shows.

*Example A-16. Chaining promises together is simpler*

```
uploadToSomeAPI()
  .then(result => updateUserInterface(result))
  .then(uiUpdateResult => checkForNewData(uiUpdateResult))
  .then(newDataResult => successCallback(newDataResult))
  .catch(errorCallback)
```

This keeps our code cleaner. It also means that we don't need to reimplement callback handling each time we write a function.



### APPENDIX B

## Deploying Your Application

Once you have built your *totally awesome* application, you'll want to get it into the hands of your users.

The process for building and deploying your production application varies by platform, and both Google and Apple periodically update the specific steps required. However, the basic process remains the same:

1. Triple-check your assets: application icon, launch screen, and so on.
2. Specify target OS versions and devices.
3. Create a release build.
4. Get your paperwork in order.
5. Create an App Store and Play Store listing, including promotional screenshots.
6. Send the app to your beta tester and solicit feedback.
7. Submit for review.
8. Release!

### Check Your Application Assets and Specify Target OS Versions and Devices

It's easy to overlook these steps during development. You'll want to make sure that you have a suitable application icon and launch screen for your application in the correct sizes and resolutions for all the devices you intend to target.

Similarly, for any images, video, or other assets utilized by your application, make sure that you have included versions appropriate to each targeted device.

### Create a Release Build

You will need to compile your application into a production-ready release build before shipping it off to your end users. This version of your application won't have debugging enabled and will include the bundled JavaScript instead of relying on the React Native packager.

For both iOS and Android, the official React Native documentation (*http://facebook.github.io/react-native/docs/running-on-device.html*) includes guidance on creating production-ready builds. 

### Complete Your Paperwork

In order to distribute your application to Android devices, you'll probably want to register with Google Play (*https://developer.android.com*). Similarly, you'll need to register for an Apple Developer account (*https://developer.apple.com*) in order to submit to the App Store.

As part of this process, you need to provide some standard information, such as contact and payment information.

### Beta Test Your Application

You'll want to test your application on a variety of devices and operating system versions. How does it perform in landscape versus portrait mode? With low battery? With a slow network? What happens when you get interrupted by push notification? 

Getting your application into the hands of real users is the best way to evaluate how your application performs in real-world scenarios. Both the Play Store and the App Store have built-in programs to facilitate distribution of your application to beta testers.

### Create a Listing

You'll need to convince people to download your application! Gather your promotional screenshots, select the appropriate category, and write a compelling description.

Once that's done, you can submit your application for review.

### Wait for Review

As web developers, we're used to having more control over our deploy processes. You may be accustomed to shipping code to production many times in a single day, where versions are usually a nonissue. With the iOS App Store and Google Play Store, deployment is more complicated, and new version releases typically require review. Review times vary from a day to a couple of weeks. Thus, it's important to take the submission and review process into account during your planning phase. 

### Release

After putting in the hard work to create your application, seeing it go live (Figure B-1) can feel exhilarating. However, releasing your application to users is just the beginning, as you'll have to support your application postrelease. Unlike the web, where you can deploy often and easily, new mobile versions take time, and have a longer lifespan. Many iOS and Android users don't have auto-updating enabled, so every version counts. And at minimum you'll need to wait for application review each time you wish to submit an update or bug fix. (For truly critical bug fixes, you can request an expedited review, but use these carefully.)

![figurebto1](./img/B-1.JPG)

*Figure B-1. The flashcard application, live on the Play Store*

And, finally: congratulation on shipping your app!



### APPENDIX C

## Working with Expo Applications

Expo is a tool that allows you to write React Native apps without using Xcode or Android Studio. Projects created with the Create React Native App tool are Expo projects.

Expo makes it very easy to develop on your physical device and removes many of the initial roadblocks to getting started with React Native. Thus, it's a great choice while you're learning to develop using React Native. 

You can read more about Expo and install the Expo mobile app at expo.io (*https://expo.oi/*).

### Ejecting from Expo

Any project that relies on custom native code (either your own, or third-party modules that instruct you to run `react-native link` to install them) will not work with Expo. Expo provides a way to "eject" from Expo into a traditional, full React Native project. Ejecting will create a full React Native project from your existing Expo application. This is a one-way migration, so you won't be able to go back to Expo afterwards.

You will also need to eject from Expo if you want more control over building and publishing your application for the iOS App Store or Google Play Store. 

More information is available in the Create React Native App documentation (*https://github.com/react-community/create-react-native-app*).