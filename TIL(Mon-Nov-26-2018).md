# TIL

# Mon Nov 26 2018

# 1. Today I Learned



# CSS Methodology 

[[CSS Methodology] SMACSS, BEM, OOCSS](http://wit.nts-corp.com/2015/04/16/3538)



## BEM *(Block, Element, Module)*

## Definition

- BEM stands for **Block Element Modifier **
- **Similar to OOP**(Object Oriented Programming)

- ID can not be used, only class name can be used
- Use the same class as `.header__navigation - secondary`



## Block

- Block is a container containing an element or element applied to the entire paragraph.
  ex) logo / login form / menu / search from / content / footer

```xhtml
<div class="header">
  <div class="menu">....</div>
  <div class="search">....</div>
</div>
```



![BEM_2](https://tarh.developpez.com/articles/2014/bonnes-pratiques-en-css-bem-et-oocss/images/bem-blocks.png)
![BEM_3](https://cdn-images-1.medium.com/max/800/1*o5vL1JKiQ0EgzfKXnxg22g.png)



## Element

- Element is a component that performs a specific function within a block. The element depends on the situation.

  Each element is connected with two underscores to create it after the block.

  ```css
  1 .header__logo { … }
  2 .header__menu { … }
  3 .header__search { … }
  4 .header__login { … }
  ```


- If block name or element name is long - connect with hyphen. (No coercion, just the rules of the project)

```css
.block-name__element-name
```



![BEM_4](https://cdn-images-1.medium.com/max/800/1*sA5DOZSjwhNg-3EIJYhF8g.png)



## Modifiers

- Modifier is a property of block or element
- This attribute changes the appearance or state of a block or element
- Add a modifier by adding "-" to the class name

```css
1 .block‐‐modifier {…}
2 .block__element--modifier {…}
```



- If tab menus are used in different styles in different areas,

  - Copy and add main attributes,
  - You can inherit attributes using @extend of sass, a preprocessor

```css
.header__navigation { 
      background: #008cba; 
      padding: 1px 0; 
      margin: 2px 0; 
}     
.header__navigation‐‐secondary { 
      @extend .header__navigation;
      
      background: #dfe0e0; 
 }	
```

- The class is long?
  - The BEM class name is specific and clear and should be easy to read in HTML
  - The class name must be clearly conveyed



※ TIP

The use of BEM makes it clear that roles and responsibilities are clearly attached to class names. Shared styles are preferred to using `@ mixin` instead of dirtying class names



---

# SCSS

## Nesting

- Used to reduce the amount of code written.

```scss
// SCSS

av {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

- In a nested code, the use of a self-selector (&) will result in missing parent information.
- When writing the same code in duplicate, you can use the self-selector (&).



## Import

CSS has an import option that lets you split your CSS into smaller, more maintainable portions. The only drawback is that each time you use `@import` in CSS it creates another HTTP request. Sass builds on top of the current CSS `@import` but instead of requiring an HTTP request, Sass will take the file that you want to import and combine it with the file you're importing into so you can serve a single CSS file to the web browser.

```scss
// base.scss

@import 'reset';

body {
  font: 100% Helvetica, sans-serif;
  background-color: #efefef;
}
```



## Mixins

Some things in CSS are a bit tedious to write, especially with CSS3 and the many vendor prefixes that exist. A mixin lets you make groups of CSS declarations that you want to reuse throughout your site. You can even pass in values to make your mixin more flexible. A good use of a mixin is for vendor prefixes.

```scss
@mixin transform($property) {
  -webkit-transform: $property;
  -ms-transform: $property;
  transform: $property;
}

.box { @include transform(rotate(30deg)); }
```

To create a mixin you use the `@mixin` directive and give it a name. We've named our mixin `transform`. We're also using the variable `$property` inside the parentheses so we can pass in a transform of whatever we want. After you create your mixin, you can then use it as a CSS declaration starting with `@include` followed by the name of the mixin.



※ TIP

`@content` acts as a space to allow the '`…`' part of `@include{…}` to come in.



## Operators

Doing math in your CSS is very helpful. Sass has a handful of standard math operators like `+`, `-`, `*`, `/`, and `%`. In our example we're going to do some simple math to calculate widths for an `aside` & `article`.

```scss
.container {
  width: 100%;
}

article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}

aside[role="complementary"] {
  float: right;
  width: 300px / 960px * 100%;
}
```





# Create React App

# > Styles and Assets

## Adding a CSS Modules Stylesheet

- The css file is transformed and the class name is created and the class name is included in the object.
- The attribute name part contains the name of the class I wrote, and the attribute value contains the converted class name.
- No need to worry about name collisions

※ TIP

Write the class name as CamelCase (to use dot notation instead of bracket notation)





# UI Framework for React

## Sementic UI React

- [Semantic UI React](https://react.semantic-ui.com/)



## Reactstrap

- [Reactstrap](https://reactstrap.github.io/)



# Storybook

- [Storybook](https://storybook.js.org/ )

- The UI Development Environment

- Tool to create demo page for components



## Loading stories dynamically


Sometimes, stories need to be loaded dynamically rather than explicitly in the Storybook config file.

For example, the stories for an app may all be inside the `src/components` directory with the `.stories.js` extension. It is easier to load all the stories automatically like this inside the `.storybook/config.js` file:

```js
import { configure } from '@storybook/react';

const req = require.context('../src/components', true, /\.stories\.js$/);

function loadStories() {
  req.keys().forEach(filename => req(filename));
}

configure(loadStories, module);
```

Storybook uses Webpack’s [require.context](https://webpack.js.org/guides/dependency-management/#require-context) to load modules dynamically. Take a look at the relevant Webpack [docs](https://webpack.js.org/guides/dependency-management/#require-context) to learn more about how to use `require.context`.

The **React Native** packager resolves all the imports at build-time, so it’s not possible to load modules dynamically. There is a third party loader [react-native-storybook-loader](https://github.com/elderfo/react-native-storybook-loader) to automatically generate the import statements for all stories.



---

# Presentational and Container Components

- [Presentational and Container Components - Medium](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)





# 2. Reference

- [ClassNames - npm](https://www.npmjs.com/package/classnames)
- [SASS(SCSS) - Official Website](https://sass-lang.com/guide)
- [Craete React App: Ch. Styles and Assets - Official Website](https://facebook.github.io/create-react-app/docs/adding-a-stylesheet)
- [Presentational and Container Components - Medium](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)

### UI Framework

- [Semantic-UI](https://semantic-ui.com/)
  - [Semantic UI React](https://react.semantic-ui.com/)
- [Bootstrap](https://getbootstrap.com/)
  - [Reactstrap](https://reactstrap.github.io/)

### Dev Tool

- [Storybook](https://storybook.js.org/ )
