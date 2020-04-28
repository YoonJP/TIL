### CHAPTER 11

## State Management in Larger Applications

In Chapter 10, we used the flashcard application as a jumping-off point to discuss the structure of larger applications. One of the common issues that React applications encounter as they grow is *state management*. React Native is no different: as our application gets larger, we can benefit from using a state management library. In this chapter, we'll look at Redux, a library for managing data flow, and integrate it with our flashcards application. We'll also integrate AsyncStorage with our Redux store.

### Using Redux to Manage State

Redux is based somewhat on the Flux data flow pattern, as well as functional programming concepts. Previous examples we've looked at in this book haven't required much in the way of data flow management. With smaller applications, communicating between components is usually a trivial issue. Consider the case where a button tap has an impact on the parent's state:

```react
	class Child extends Component {
    render() {
      <TouchableOpacity onPress={this.props.onPress}>
        <Text>Child Component</Text>
      </TouchableOpacity>
    }
  }
```

By pressing a callback from the parent to the child, we can alert the parent about interactions with the child: 

```react
	class Parent extends Component {
    constructor(props) {
      super(props);
      this.initialState = { numTaps: 0 };
    }
    
    _handlePress = () => {
      this.setState({numTaps: this.state.numTaps + 1});
    }
    
    render() {
      <Child onPress={this._handlePress}/>
    }
  }
```

For simple use cases, this pattern works just fine.

Our need for a more robust data flow architecture becomes apparent when we consider a more complex interaction. What happens when a component much farther down the component tree needs to impact an application state located on a higher level? It's very easy to end up with a tangle of spaghetti, and to spend tedious time stringing callbacks through your code. Managing active routes, handling user interactions, fetching data from the server, animating changes—as you add more state to your application, the complexity grows, and cascading updates can be triggered in unpredictable ways. 

Redux is one of many libraries designed to make it easier to manage your application's state, with the goal of making state changes predictable and easy to manage.

In Redux, state is located in a single object,  in a single *store*, which acts as the sole source of truth. Components that need to render based on state can *connect* to that store and receive the state as *props*. Components cannot modify state directly. 

Changes to state are triggered by a set of predefined *actions*. A single *reducer* combines the previous state and information from the action in order to calculate the new state. Thus, logic about how your state can change, and when, is centralized in one easy-to-debug location.

All of this will likely make more sense in practice than in theory. Let's install Redux and look at how to add it to our flashcard application. In addition to the `redux` package, we will want to install the `react-redux` package, which contains the React bindings for redux. 

​	**`npm install --save redux react-redux`**

### Actions

First things first: let's define what types of *actions* can result in state changes. We're going to create some string constants to represent the different types of actions (see Example 11-1).

*Example 11-1. src_checkpoint_03/actions/types.js*

```react
export const ADD_DECK = "ADD_DECK";
export const ADD_CARD = "ADD_CARD";
export const REVIEW_DECK = "REVIEW_DECK";
export const STOP_REVIEW = "STOP_REVIEW";
export const NEXT_REVIEW = "NEXT_REVIEW";
```

Each of these action types represents a user interaction and covers the basic functionality of our application: adding cards or decks, or starting or stopping a review.

An *action* in Redux is an object that contains a key named *type*, and some optional extra data. We need to add some *action creators* to create these objects (see Example 11-2). While we could theoretically skip having a separate file with action creators, centralizing this code will help keep our React components clean, and gives us a single file to glance at to find action definitions.

*Example 11-2. src_checkpoint_03/actions/creators.js*

```react
import {
  ADD_DECK,
  ADD_CARD,
  REVIEW_DECK,
  STOP_REVIEW,
  NEXT_REVIEW
} from "./types";

import Card from "../data/Card";
import Deck from "../data/Deck";

export const addDeck = name => {
  return { type: ADD_DECK, data: new Deck(name) };
};

export const addCard = (front, back, deckID) => {
  return { type: ADD_CARD, data: new Card(front, back, deckID) };
};

export const reviewDeck = deckID => {
  return { type: REVIEW_DECK, data: { deckID: deckID } };
};

export const stopReview = () => {
  retunr { type: STOP_REVIEW, data: {} };
};

export const nextReviwe = () => {
  return { type: NEXT_REVIEW, data: {} };
};
```

In many ways, these action creators act as convenience functions. For example, the `addDeck` action creator takes a deck name as a parameter and then handles the actual construction of a `Deck`.

### Reducers

Actions represent things that happened in your application. *Reducers* describe how your application state changes in response to actions. A reducer is a "pure function": it has no side effects, and its return value is determined only by its inputs. (Don't call `Math.random` in a reducer.)

The simplest reducer we could write would look like:

```react
	const reducer = (state = {}, action) => {
    return state;
  }
```

Our state is going to contain two items: an array of decks and information about the current review. The default state will look like this:

```react
	decks: [],
  currentReview: {
    deckID = null,
		questions = [],
    currentQuestionIndex = 0
  }
```

Let's start writing our first reducer by looking at the `ADD_DECK` action. Looking back at *actions/creator.js*, we se the following action:

```react
	{
    type: ADD_DECK,
    data: new Deck(name)
  }
```

If we want to write a reducer for the `decks` key, the signature needs to look like:

```react
	const deckReducer = (state = [], action) => {
    // returns some state
  }
```

We want to add the new deck from our action to the existing state, so let's implement the `deckReducer`.

```react
	const deckReducer = (state = [], action) => {
    switch (action.type) {
      case ADD_DECK:
        return state.concat(action.data);
    }
    return state;
  }
```

First, we need a `switch` statement based on the action's type. We're only handling the `ADD_DECK` action for now. In all other cases we return the original state, unmodified. This is very important—don't forget to handle default case!

Then, if the action type is in fact `ADD_DECK`, we concatenate the new deck to our existing deck state and return it. 

Now let's implement the rest of the `deckReducer` (see Example 11-3).

*Example 11-3. src_checkpoint_03/reducers/decks.js*

```react
import { ADD_DECK, ADD_CARD } from "../actions/types";

function decksWithNewCard(oldDecks, card) {
	return oldDecks.map(deck => {
    if (deck.id === card.deckID) {
      deck.addCard(card);
      return deck;
    } else {
      return deck;
    }
  });  
}

const reducer = (state = [], action) => {
  console.warn("Changes are not persisted to disk");
  
  switch (action.type) {
    case ADD_DECK:
      return state.concat(action.data);
    case ADD_CARD:
      return decksWithNewCard(state, action.data);
	}
  return state;
};

export default reducer;
```

Next, let's look at the reviews reducer (Example 11-4). This reducer will handel the `REVIEW_DECK`, `NEXT_REVIEW`, and `STOP_REVIEW` actions. Handling `STOP_REVIEW` is simplest: we'll replace the state with the default state. For `NEXT_REVIEW`, we increment the review index. Handling `REVIEW_DECK` is somewhat more complex because we have to take a deck of cards and generate questions based on it.

*Example 11-4. src_checkpoint_03/reducers/reviews.js*

```react
import { mkReviews } from "./../data/QuizCardView";
import { REVIEW_DECK, NEXT_REVIEW, STOP_REVIEW } from "./../actions/types";

export const mkReviewState = (
  deckID = null,
  questions = [],
  currentQuestionIndex = 0
) => {
  return { deckID, questions, currentQuestionIndex };
};

function findDeck(decks, id) {
  return decks.find(d => {
    return d.id === id;
  });
}

function generateReviews(deck) {
  return mkReviewState(deck.id, mkReviews(deck.cards), 0);
}

function nextReview(state) {
  return mkReviewState(
    state.deckID,
    state.questions,
    state.currentQuestionIndex + 1
  );
}

const reducer = (state = mkReviewState(), action, decks) => {
	switch (action.type) {
    case REVIEW_DECK:
      return generateReviews(findDeck(decks, action.data.deckID));
    case NEXT_REVIEW:
      return nextReview(state);
    case STOP_REVIEW:
      return mkReviewState();
  }
  retunr state;
};

export default reducer;
```

Note that this reducer depends on deck information, so its signature is slightly different than the `decksReducer`.

Now let's wire them up together. In Redux, you only connect a single reducer to your store, so we need to combine these into one reducer (see Example 11-5).

*Example 11-5. src_checkpoint_03/reducers/index.js*

```react
import { MockDecks, MockCards } from "./../data/Mocks";

import DecksReducers from "./decks";
import ReviewReducer, { mkReviewState } from "./reviews";

const initialState = () => {
  return { decks: MockDecks, currentReview: mkReviewState() };
};

export const reducer = (state = initalState(), action) => {
  let decks = DecksReducer(state.decks, action);
  
  return {
    decks: decks,
    currentReview: ReviewReducer(state.currentReview, action, decks)
  };
};
```

Now that we've written some Redux-specific code, the next step is to integrate it into our actual application.

#### Connecting Redux

Remember how we said that state is located in a single Redux *store*? Let's open up *components/Flashcard.js*, which is the root component for our application, and create that store. 

First we need to import the `createStore` method from `redux`, as well as the reducer that we just created in *reducer/index.js*. Then we can create the store.

```react
	import { createStore } from "redux";
	import { reducer } from "../reducers/index";

	let store = createStore(reducer);
```

Next, in order to use this store from our application, we need to add a `<Provider>` component.

Wrapping your application's root component in a `<Provider>` makes the Redux store available to any component at any part of the component hierarchy. Remember, state in Redux is read-only, so there's no risk of complications from *reading* state at any point in the component hierarchy. `<Provider>` is part of the `react-redux` package. 

Let's wire that in. Example 11-6 shows the full component file after we integrate our Redux store.

*Example 11-6. src_checkpoint_03/components/Flashcards.js*

```react
import React, { Component } from "react";
import { StyleSheet, View } from "react-native";
import { StackNavigator } from "react-navigation";
import { createStore } from "redux";
import { Provider } from "react-redux";

import { reducer } from  "../reducers/index";

import Logo from "./Header/Logo";
import DeckScreen from "./DeckScreen";
import NewCardScreen from "./NewCardScreen";
import ReviewScreen from "./ReviewScreen";

let store = createStore(reducer);

let headerOptions = {
  headerStyle: { backgroundColor: "#FFFFFF" },
  headerLeft: <Logo />
};

const Navigator = StackNavigator({
  Home: { screen: DeckScreen, navigationOptions: headerOptions },
  Review: { screen: ReviewScreen, navigationOptions: headerOptions },
  CardCreation: {
    screen: NewCardScreen,
    path: "createCard/:deckID",
    navigationOptions: headerOptions
  }
});

class App extends Component {
  render() {
    return (
      <Provider store={store}>
        <Navigator />
      </Provider>
    );
  }
}

export default App;
```

Now that we've integrated Redux, let's use it to render some data. We'll start by modifying the `<DecksScreen>` components to display decks based on the contents of the Redux store.

In order to *connect* a given component to our Redux store, we use the `react-redux` bindings.

```react
	import { connect } from "react-redux"
```

Then we need to define two functions: `mapStateToProps` and `mapDispatchToProps`.

`mapStateToProps` describes how the Redux store's state will be provided to this component as props. Our state includes an array of decks. We'll want to calculate the `counts` here, too.

```react
	const mapStateToProps = state => {
    return {
      decks: state.decks,
      counts: state.decks.reduce(
        (sum, deck) => {
          sum[deck.id] = deck.cards.length;
          return sum;
        },
        {}
      )
    };
  };
```

Meanwhile, `mapDispatchToProps` defines the props that a component will receive, which can be used to dispatch actions. We need to import our action creators and then invoke them from here.

```react
	import { addDeck, reviewDeck } from "./../../actions/creators";
	...
  const mapDispatchToProps = dispatch => {
    return {
      createDeck: deckAction => {
        dispatch(deckAction);
      },
      reviewDeck: deckID => {
        dispatch(reviewDeck(deckID));
      }
    };
  };
```

Finally, we need to call `connect()` to create a Redux-connected component.

```react
	export default connect(mapStateToProps, mapDispatchToProps)(DecksScreen);
```

Pulling it all together, we can use these new props (`reviewDeck`, `createDeck`, `decks`, and `counts`) in our component. Now, the `<DecksScreen>` will render based on props received from Redux, and it will also dispatch Redux action instead of modifying state directly (see Example 11-7).

*Example 11-7. src_checkpoint_03/components/DeckScreen/index.js*

```react
import React, { Component } from "react";
import { View } from "react-native";

import { connect } from "react-redux";

import  { MockDecks } from "./../../data/Mocks";
import { addDeck, reviewDeck } from "./../../actions/creators";
import Deck from "./Deck";
import DeckCreation from "./DeckCreation";

class DecksScreen extends Component {
  static displayName = "DecksScreen";

	static navigationOptions = { title: "All Decks" };

	_createDeck = name => {
    let createDeckAction = addDeck(name);
    this.props.createDeck(createDeckAction);
    this.props.navigation.navigate("CardCreation", {
      deckID: createDeckAction.data.id
    });
  };

	_addCards = deckID => {
    this.props.navigation.navigate("CardCreation", { deckID: deckID });
  };

	_review = deckID => {
    this.props.reviewDeck(deckID);
    this.props.navigation.navigate("Review");
  };

	_mkDeckViews() {
    if (!this.props.decks) {
      return null;
    }
    
    return this.props.decks.map(deck => {
      return (
        <Deck
        	deck={deck}
          count={this.props.counts[deck.id]}
          key={deck.id}
          add={() => {
            this._review(deck.id);
					}}
        />
      );
    });
  }

	render() {
    return (
      <View>
        {this._mkDeckViews()}
        <DeckCreation create={this._createDeck} />
      </View>
    );
  }
}

const mapDispatchToProps = dispatch => {
  return {
    createDeck: deckAction => {
      dispatch(deckAction);
    },
    reviewDeck: deckID => {
      dispatch(reviewDeck(deckID));
    }
  };
};

const mapStateToProps = state => {
  return {
    decks: state.decks,
    counts: state.dekcs.reduce(
      (sum, deck) => {
        sum[deck.id] = deck.cards.length;
        return sum;
      },
      {}
    )
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(DecksScreen);
```

In general, when you are converting to Redux or a similar library, replacing access to or mutating `this.state` is a common pattern. The more your components rely on props instead of state, the easier it is to manage growing complexity in your application. 

We need to make similar updates to the `<NewCardScreen>` and `<ReviewScreen>` components as well; see Example 11-8 and 11-9, respectively. As we did with `<DecksScreen>`, we implement `mapDispatchToProps` and `mapStateToProps` for each of them.

*Example 11-8. src_checkpoint_03/components/NewCardScreen/index.js*

```react
import React, { Component } from "react";
import { StyleSheet, View } from "react-native";

import DeckModel from "./../../data/Deck";
import { addCard } from "./../../actions/creators";
import { connect } from "react-redux";

import Button from "../Button";
import LabeledInput from "../LabeledInput";
import NormalText from "../NormalText";
import colors from "./../../styles/colors";

class NewCard extends Component {
  static navigationOptions = { title: "Create Card" };
	
	static initialState = { front: "", back: "" };

	constructor(props) {
    super(props);
    this.state = this.initialState;
	}

	_deckID = () => {
    return this.props.navigation.state.params.deckID;
  };
	
	_handleFront = text => {
    this.setState({ front: text });
  };
	
	_handleBack = text => {
		this.setState({ back: text });
  };

	_createCard = () => {
    this.props.createCard(this.state.front, this.state.back, this._deckID());
    this.props.navigation.navigate("CardCreation", { deckID: this._deckID() });
  }
  
  _reviewDeck = () => {
    this.props.navigation.navigate("Review");
  };

	_doneCreating = () => {
    this.props.navigation.navigate("Home");
  };

	render() {
    return (
      <View>
        <LabeledInput
          label="Front"
          clearOnSubmit={false}
          onEntry={this._handleFront}
          onChange={this._handleFront}
        />
        <LabeledInput
          label="Back"
          clearOnSumbit={false}
          onEntry={this._handleBack}
          onChange={this._handleBack}
        />
        <Buttton style={styles.createButton} onPress={this._createCard}>
          <NormalText>Create Card</NormalText>
        </Buttton>
        
        <View>
          <Button style={styles.secondaryButton} onPress={this._doneCreating}>
            <NormalText>Done</NormalText>
          </Button>
          
          <Button style={styles.secondaryButton} onPress={this._reviewDeck}>
            <NormalText>Review Deck</NormalText>
          </Button>
        </View>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  createButton: { backgroundColor: colors.green },
  secondaryButton: { backgroundColor: colors.blue },
  buttonRow: { flexDirection: "row" }
});

const mapStateToProps = state => {
  return { decks: state.decks };
};

const mapDispatchToProps = dispatch => {
  return {
    createCard: (front, back, deckID) => {
      dispatch(addCard(front, back, deckID));
    }
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(NewCard);
```

*Example 11-9. src_checkpoint_03/components/ReviewScreen/index.js*

```react
import React, { Component } from "react";
import { StyleSheet, View } from "react-native";

import { connect } from "react-redux";
import ViewCard from "./ViewCard";
import { mkReviewSummary } from "./ReviewSummary";
import colors from "./../../styles/colors";
import { reviewCard, nextReview, stopReview } from "./../../actions/creators";

class ReviewScreen extends Component {
  static displayName = "ReviewScreen";

	static navigationOptions = { title: "Review" };

	constructor(props) {
    super(props);
    this.state = { numReviewed: 0, numCorrect: 0 };
	}

	onReview = correct => {
    if (correct) {
      this.setState({ numCorrect: this.state.numCorrect + 1 });
    }
    this.setState({ numReviewed: this.state.numReviewed + 1 });
  };

	_nextReview = () => {
    this.props.nextReview();
  };

	_quitReviewing = () => {
    this.props.stopReview();
    this.props.navigation.goBack();
  };

	_contents() {
    if (!this.props.reviews || this.props.reviews.length === 0) {
      return null;
    }
    
    if (this.props.currentReview < this.props.reviews.length) {
      return (
        <ViewCard
          onReview={this.onReview}
          continue={this._nextReview}
          quit={this._quitReviewing}
          {...this.props.reviews[this.props.currentReview]}
        />
      ); 
    } else {
      let percent = this.state.numCorrect / this.state.numReviewed;
      return mkReviewSummary(percent, this._quiteReviewing);
    }
  }

	render() {
    return (
    	<View style={styles.container}>
        {this._contents()}
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: { backgroundColor: colors.blue, flex: 1, paddingTop: 24 }
});

const mapDispatchToProps = dispatch => {
  return {
    nextReview: () => {
      dispatch(nextReview());
    },
    stopReview: () => {
      dispatch(stopReview());
    }
  };
};

const mapStateToProps = state => {
  return {
    reviews: state.currentReview.questions,
    currentReview: state.currentReview.currentQuestionIndex
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(ReviewScreen);
```

#### Persisting Data with AsyncStorage

Right now, our flashcard application's state isn't persisted, so if we add new decks or cards and then restart the app, our data is lost. Let's fix this by saving the application's state with `AsyncStorage`.

This is an example of how Redux can really shine: because our state management logic is centralized, making this change is simpler than it would otherwise be.

We'll start by adding a file that handles read/write logic for persisting our state to disk; see Example 11-10. Remember, `AsyncStorage.getItem` and `AsyncStorage.setItem` are both asynchronous APIs.

*Example 11-10. src_checkpoint_04/storage/decks.js*

```react
import { AsyncStorage } from "react-native";
import Deck from "./../data/Deck";
export const DECK_KEY = "flashcards:decks";
import { MockDecks } from "./../data/Mocks";

async function read(key, deserializer) {
  try {
    let val = await AsyncStorage.getItem(key);
    if (val !== null) {
      let readValue = JSON.parse(val).map(serialized => {
        return deserializer(serialized);
      });
      return readValue;
    } else {
      console.info(`${key} not found on disk.`);
      return [];
    }
  } catch(error) {
    console.warn("AsyncStorage error: ", error.message);
  }
}

async function write(key, item) {
  try {
    await AsyncStorage.setItem(key, JSON.stringify(item));
  } catch (error) {
    console.error("AsyncStorage error: ", error.message);
  }
}

export const readDecks = () => {
	return read(DECK_KEY, Deck.fromObject);
};

export const writeDecks = decks => {
  return write(DECK_KEY, decks);
};

// For debug/test purposes.
const replaceData = writeDecks(MockDecks);
```

Remember that our Redux state has two elements: `decks` and `currentReview`. Because `currentReview` is transient information, we only need to worry about saving `decks`.

Now that we have an easy way of reading and writing our decks to `AsyncStorage`, let's add a new action type, `LOAD_DATA`, to *actions/types.js*, as shown in Example 11-11.

*Example 11-11. Adding a new type to src_checkpoint_04/actions/types.js*

```react
export const loadData = data => {
  return { type: LOAD_DATA, data: data };
};
```

Next, update *Flashcards.js* to load data from disk after our store is created.

```react
	import { readDecks } from "../storage/decks";
	import  { loadData } from "../actions/creators";

	...
  
  let store = createStore(reducer);

	// On application start, read saved state from disk.
	readDecks().then(desks => {
    store.dispatch(loadData(decks));
  });
```

Now that we have dispatched the action, we need to update our deck reducer to handle the `LOAD_DATA` action. Additionally, when handling the `ADD_CARD` or `ADD_DECK` actions, this reducer should save the deck state (see Example 11-13).

*Example 11-13. Updating src_checkpoint_04/reducers/decks.js to save state*

```react
import { ADD_DECK, ADD_CARD, LOAD_DATA } from "../actinos/types";
import DECK from "./../data/Deck";
import { writeDecks } from "./../storage/decks";

function decksWithNewCard(oldDecks, card) {
  let newState = oldDecks.map(deck => {
    if (deck.id === card.deckID) {
      dekc.addCard(card);
      return deck;
    } else {
      return deck;
    }
  });
  saveDecks(newState);
  return newState;
}

function saveDecks(state) {
  writeDecks(state);
  return state;
}

const reducer = (state = [], action) => {
  switch (action.type) {
		case LOAD_DATA;
      return action.data;
    case ADD_DECK;
      let newState = state.concat(action.data);
      saveDecks(newState);
      return newState;
    case ADD_CARD;
      return decksWithNewCard(state, action.data);
      
  }
  return  state;
};

export default reducer;
```

And... that's it! Because state is managed by Redux, we can be confident that by modifying our deck reducer, we've ensured that all relevant state changes will persisted to AsyncStorage.

#### Summary and Homework

A common critique of Redux—and similar state management libraries—is that it adds significant boilerplate to your application. Indeed, we had to write several new files in order to integrate Redux into our flashcard application. However, by expressing state relationships explicitly rather than mutating state locally, this "boilerplate" makes existing complexity much more manageable. It's harder to write state-based bugs with Redux! You also get some nice bonus like time traveling debugging. Plus, as we saw when integrating AsyncStorage, making further changes to your application becomes much easier.

Which particular state management library you use doesn't matter so much; there are many reasonable ways to structure a large application. However, as with any large React application, if you don't plan for state management, eventually you will probably start to encounter bugs related to state mutations and have difficulty making changes to existing components. This is a good sign that you need to put more planning into your state and data flow management.

The flashcard application is meant to serve as a reference. In many ways, it's a "minimum viable project," and there are plenty of ways it could be improved. That being said, there's still plenty to explore in the codebase, and I encourage you to dig into it. 

If you want to get some more practice working within the context of React Native, check out the GitHub repository and try extending the flashcard application. Here are some ideas to get you started:

* Add the ability to delete decks
* Add a screen where you can view all cards in a deck
* Display statistics about review performance over time
* Experiment with different styles