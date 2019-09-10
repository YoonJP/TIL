# Mobile App Development with React Native

------

# Lecture 5: User Input, Debugging

## Previous Lecture

- ScrollView
- FlatList
- SectionList
- Controlled vs Uncontrolled components
- TextInput



## User Input

- Controlled vs uncontrolled components
  - Where is the source of truth for the value of an input?
- React recommends always using controlled components
- Pass value and onChangeText props

https://facebook.github.io/react-native/docs/textinput.html



## Handling multiple inputs

*  <form> exists in HTML, but not in React Native

- With controlled components, we maintain an object with all inputs' values
- We can define a function that handles the data to submit



## Validating Input

- Conditionally set state based on input value
- Validate form before submitting
- Validate form after changing single input value
  - `this.setState()` can take a callback as the second argument
  - `componentDidUpdate()`



## KeyboardAvodingView

- Native component to handle avoiding the virtual keyboard
- Good for simple/short forms
  - The view moves independent of nay its child TextInputs



## Debugging

- React errors/warnings
- Chrome Developer Tools (devtools)
- React Native Inspector
- react-devtools



## React Errors and Warnings

- Errors show up as full page alerts
  - Trigger with console.error()
- Warnings are yellow banners
  - Trigger with console.warn()
  - Does not appear in production mode



## Chrome Devtools

- Google Chrome has amazing developer tools (debugger)
- We can run the JavaScript inside a Chrome tab
  - Separate threads for native and JavaScript
  - Communicate asynchronously through bridge
  - No reason that the JavaScript needs to be run on device



## React Native Inspector

- Analogous to the Chrome element inspector
- Allows you to inspect data associated with elements, such as margin, padding, size, etc.
- Does not allow you to live-edit elements 



## react-devtools

- "Inspect the React component hierarchy, including component props and state."
- Install with `npm install -g react-devtools`
- Run with `react-devtools`
- Allows us to make live-edits to style, props, etc.

http://github.com/facebook/react-devtools

 

## External Libraries

- Libraries are code written outside the context of your project that you can bring into your project
- Since React Native is just JavaScript, you can add any JavaScript library
- Install using `npm install <library>`
  - Use the --save flag for npm@"<5"
  - Use the -g flag to install things globally
- Import into your project
  - import React from 'react'



---

## Source Code

- AddContactForm.js

```js
import React from 'react'
import {Button, KeyboardAvoidingView, StyleSheet, TextInput, View} from 'react-native'
import {Constants} from 'expo'

const styles = StyleSheet.create({
  container: {
    backgroundColor: '#fff',
    paddingTop: Constants.statusBarHeight,
  },
  input: {
    borderWidth: 1,
    borderColor: 'black',
    minWidth: 100,
    marginTop: 20,
    marginHorizontal: 20,
    paddingHorizontal: 10,
    paddingVertical: 5,
    borderRadius: 3,
  },
})

export default class AddContactForm extends React.Component {
  state = {
    name: '',
    phone: '',
    isFormValid: false,
  }

  componentDidUpdate(prevProps, prevState) {
    if (this.state.name !== prevState.name || this.state.phone !== prevState.phone) {
      this.validateForm()
    }
  }

  getHandler = key => val => {
    this.setState({[key]: val})
  }

  handleNameChange = this.getHandler('name') // val => { this.setState({name: val}) }
  handlePhoneChange = this.getHandler('phone')

    /*
  handleNameChange = name => {
    this.setState({name})
  }
  */

  handlePhoneChange = phone => {
    if (+phone >= 0 && phone.length <= 10) {
      this.setState({phone})
    }
  }

  validateForm = () => {
    console.log(this.state)
    const names = this.state.name.split(' ')
    if (+this.state.phone >= 0 && this.state.phone.length === 10 && names.length >= 2 && names[0] && names[1]) {
      this.setState({isFormValid: true})
    } else {
      this.setState({isFormValid: false})
    }
  }

  validateForm2 = () => {
    if (+this.state.phone >= 0 && this.state.phone.length === 10 && this.state.name.length >= 3) {
      return true
    } else {
      return false
    }
  }


  handleSubmit = () => {
    this.props.onSubmit(this.state)
  }

  render() {
    return (
      <KeyboardAvoidingView behavior="padding" style={styles.container}>
        <TextInput
          style={styles.input}
          value={this.state.name}
          onChangeText={this.getHandler('name')}
          placeholder="Name"
        />
        <TextInput
          keyboardType="numeric"
          style={styles.input}
          value={this.state.phone}
          onChangeText={this.getHandler('phone')}
          placeholder="Phone"
        />
        <Button title="Submit" onPress={this.handleSubmit} disabled={!this.state.isFormValid} />
      </KeyboardAvoidingView>
    )
  }
}
```



- App.js

```js
import React from 'react';
import { Button, FlatList, ScrollView, StyleSheet, Text, View } from 'react-native';
import {Constants} from 'expo'

import contacts, {compareNames} from './contacts'
import ScrollViewContacts from './ScrollViewContacts'
import FlatListContacts from './FlatListContacts'
import SectionListContacts from './SectionListContacts'
import AddContactForm from './AddContactForm'

export default class App extends React.Component {
  state = {
    showContacts: true,
    showForm: false,
    contacts: contacts,
  }

  addContact = newContact => {
    this.setState(prevState => ({showForm: false, contacts: [...prevState.contacts, newContact]}))
  }

  toggleContacts = () => {
    this.setState(prevState => ({showContacts: !prevState.showContacts}))
  }

  sort = () => {
    this.setState(prevState => ({contacts: prevState.contacts.sort(compareNames)}))
  }

  showForm = () => {
    this.setState({showForm: true})
  }

  render() {
    if (this.state.showForm) return <AddContactForm onSubmit={this.addContact} />
    return (
      <View style={styles.container}>
        <Button title="toggle contacts" onPress={this.toggleContacts} />
        <Button title="add contact" onPress={this.showForm} />
        {this.state.showContacts && <SectionListContacts contacts={this.state.contacts} />}
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    paddingTop: Constants.statusBarHeight,
  },
});
```





>**NOTE**
>
>- `...` (three dots) notation
>
>```js
>addContact = newContact => {
>	this.setState(prevState => ({contacts: [...prevState.contacts, newContact]}))
>}
>```
>
>
>
>- `+` (plus) notation 
>   - one way to check whether if the input is number
>
>```js
>handlePhoneChange = phone => {
>	if (+phone >= 0 && phone.length <= 10) {
> this.setState({phone})
>}
>}
>```
>
>
>
>- getHandler
>
>```js
>getHandler = key => val => {
>this.setState({[key]: val})
>}
>
>handleNameChange = this.getHandler('name') // val => {this.setState({name: vale})}
>handlePhoneChange = this.getHandler('phone')
>
>/*
>handleNameChange = name => {
>	this.setState({name})
>}
>*/
>```
>

