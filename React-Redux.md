# React Redux Tutorial for Beginners: The Definitive Guide (2018)

From: https://www.valentinog.com/blog/react-redux-tutorial-beginners/#React_Redux_tutorial_who_this_guide_is_for



### What is the **state**?

A **stateful React component** is a [Javascript ES6 class](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes).



```react
import React, { Component } from "react";
class ExampleComponent extends Component {
  constructor() {
    super();
    this.state = {
      articles: [
        { title: "React Redux Tutorial for Beginners", id: 1 },
        { title: "Redux e React: cos'è Redux e come usarlo con React", id: 2 }
      ]
    };
  }
  render() {
    const { articles } = this.state;
    return <ul>{articles.map(el => <li key={el.id}>{el.title}</li>)}</ul>;
  }
}
```



We could describe the initial state as a plain JavaScript object:

1. ```react
    1. var state = {
    2.   buttonClicked: 'no',
    3.   modalOpen: 'no'
    4. }
    ```


And when the user clicks the button we have:

1. ```react
    1. var state = {
    2.   buttonClicked: 'yes',
    3.   modalOpen: 'yes'
    4. }
    ```



### What problem does Redux solve?

 It helps giving **each React component** the **exact piece of state** it needs. Redux holds up the **state** within a **single location**. Also with Redux the **logic for fetching and managing the state** lives **outside React**.



### **Redux as an investment**, not as a cost.



## Redux

### Install

into your React development environment

```bash
npm i redux --save-dev
```

create a directory for the store:

```bash
mkdir -p src/js/store
```

create a new file named index.jsin src/js/storeand finally initialize the store:

```react
// src/js/store/index.js

import { createStore } from "redux";
import rootReducer from "../reducers/index";

const store = createStore(rootReducer);

export default store;
```

createStore is the function for creating the Redux store.

createStore takes a reducer as the first argument, rootReducer in our case.

You may also pass an initial state to createStore. But most of the times you don’t have to. Passing an initial state is useful for server side rendering. Anyway, **the state comes from reducers**.



## EXAMPLE

Create a directory for the root reducer:

```bash
mkdir -p src/js/reducers
```

Then create a new file named index.js in the src/js/reducers:

```js
// src/js/reducers/index.js

const initialState = {
  articles: []
};

const rootReducer = (state = initialState, action) => state;

export default rootReducer;
```



## Redux actions

It is a best pratice to **wrap every action within a function**. Such function is an **action creator**.

Create a directory for the actions:

```bash
mkdir -p src/js/actions
```

Then create a new file named index.jsin src/js/actions:

```
// src/js/actions/index.js

export const addArticle = article => ({ type: "ADD_ARTICLE", payload: article });
```

So, the **type property** is nothing more than a string.

* The reducer will use that string to determine how to calculate the next state.

Since strings are prone to typos and duplicates it’s **better to have action types declared as constants**.

* This approach helps **avoiding errors that will be difficult to debug**.



Create a new directory:

```bash
mkdir -p src/js/constants
```

Then create a new file named action-types.jsinto the src/js/constants:

```react
// src/js/constants/action-types.js

export const ADD_ARTICLE = "ADD_ARTICLE";
```

Now open up again src/js/actions/index.jsand update the action to use action types:

```js
// src/js/actions/index.js

import { ADD_ARTICLE } from "../constants/action-types";

export const addArticle = article => ({ type: ADD_ARTICLE, payload: article });
```



## Refactoring the reducer

Takes  **two parameters**: the **state** and the **action**.

```react
const initialState = {articles: []};

function exampleReducer (state = initialState, action){
    if (action.type === "BUTTON_CLICKED"){
        return Object.assign({}, state),{
            articles: state.articles.concat(action.payload)
        });
    }
}
```

When the action type matches a case clause the **reducer calculates the next state** and **returns a new object**

```react
// ...
  switch (action.type) {
    case ADD_ARTICLE:
      return { ...state, articles: [...state.articles, action.payload] };
    default:
      return state;
  }
// ...
```

> There are two key points for **avoiding mutations in Redux**:
>
> - [Using concat(), slice(), and …spread](https://egghead.io/lessons/react-redux-avoiding-array-mutations-with-concat-slice-and-spread) for arrays
> - [Using Object.assign() and …spread](https://egghead.io/lessons/react-redux-avoiding-object-mutations-with-object-assign-and-spread) for objects



The **object spread operator** is still in stage 3. Install [Object rest spread transform](https://babeljs.io/docs/plugins/transform-object-rest-spread/) to **avoid a SyntaxError Unexpected token** when using the object spread operator in Babel:

```bash
npm i --save-dev babel-plugin-transform-object-rest-spread
```



## Redux store methods

> The most important methods are:
>
> - [getState](https://redux.js.org/docs/api/Store.html#getState) for accessing the current state of the application
> - [dispatch](https://redux.js.org/docs/api/Store.html#dispatch) for dispatching an action
> - [subscribe](https://redux.js.org/docs/api/Store.html#subscribe) for listening on state changes



Create src/js/index.jsand update the file with the following code:

```js
import store from "../js/store/index";
import { addArticle } from "../js/actions/index";

window.store = store;
window.addArticle = addArticle;
```

Open up src/index.js as well, clean up its content and update it as follows:

```js
import index from "./js/index"
```

Now run webpack dev server (or Parcel) with:

```bash
npm start
```

* head over http://localhost:8080/ and open up the console with F12.

* in Console write

    ```js
    store.getState()
    ```

    Output:

    1. ```js
        {articles: Array(0)}
        ```


Dispatching an action means notifying the store that we want to change the state.

Register the callback with:

```js
store.subscribe(() => console.log('Look ma, Redux!!'))
```



To **change the state in Redux we need to dispatch an action**. To dispatch an action you have to call the [dispatch](https://redux.js.org/docs/api/Store.html#dispatch) method.

```js
store.dispatch( addArticle({ name: 'React Redux Tutorial for Beginners', id: 1 }) )
```



![](/Users/samantha/Documents/TutorialsNotes/images/store-redux.png)



## Connecting React with Redux

Run

```bash
npm i react-redux --save-dev
```



**What does mapStateToProps do** in react-redux? mapStateToProps does exactly what its name suggests: it **connects a part of the Redux state** to the [props of a React component](https://reactjs.org/docs/components-and-props.html). By doing so a connected React component will have access to the exact part of the store it needs.



**What does mapDispatchToProps** do in react-redux? mapDispatchToProps does something similar, but for actions. **mapDispatchToProps connects Redux actions to React props**. This way a connected React component will be able to dispatch actions.



## App

Create a directory for holding the components:

1. mkdir -p src/js/components

and a new file named App.jsinside src/js/components:

```js
// src/js/components/App.js
import React from "react";
import List from "./List";

const App = () => (
  <div className="row mt-5">
    <div className="col-md-4 offset-md-1">
    <h2>Articles</h2>
      <List />
    </div>
  </div>
);

export default App;
```

Take moment and look at the component without the markup:

```js
import React from "react";
import List from "./List";

const App = () => (
      <List />
);

export default App;
```

then move on to creating **List**.



## List component and Redux state

Create a new file named List.jsinside src/js/components. It should look like the following:

```js
// src/js/components/List.js

import React from "react";
import { connect } from "react-redux";

const mapStateToProps = state => {
  return { articles: state.articles };
};

const ConnectedList = ({ articles }) => (
  <ul className="list-group list-group-flush">
    {articles.map(el => (
      <li className="list-group-item" key={el.id}>
        {el.title}
      </li>
    ))}
  </ul>
);

const List = connect(mapStateToProps)(ConnectedList);

export default List;
```

The List component receives the prop articleswhich is a copy of the articlesarray. Such array lives inside the Redux state we created earlier. It comes from the reducer:

```js
const initialState = {
  articles: []
};

const rootReducer = (state = initialState, action) => {
  switch (action.type) {
    case ADD_ARTICLE:
      return { ...state, articles: [...state.articles, action.payload] };
    default:
      return state;
  }
};
```

Then it’s a matter of using the prop inside JSX for generating a list of articles:

```js
{articles.map(el => (
  <li className="list-group-item" key={el.id}>
    {el.title}
  </li>
))}
```



## Form component and Redux actions

**Not every piece of the application’s state should go inside Redux.**

Create a new file named Form.jsinside src/js/components. It should look like the following:

```js
// src/js/components/Form.js
import React, { Component } from "react";
import { connect } from "react-redux";
import uuidv1 from "uuid";
import { addArticle } from "../actions/index";

const mapDispatchToProps = dispatch => {
  return {
    addArticle: article => dispatch(addArticle(article))
  };
};

class ConnectedForm extends Component {
  constructor() {
    super();

    this.state = {
      title: ""
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({ [event.target.id]: event.target.value });
  }

  handleSubmit(event) {
    event.preventDefault();
    const { title } = this.state;
    const id = uuidv1();
    this.props.addArticle({ title, id });
    this.setState({ title: "" });
  }

  render() {
    const { title } = this.state;
    return (
      <form onSubmit={this.handleSubmit}>
        <div className="form-group">
          <label htmlFor="title">Title</label>
          <input
            type="text"
            className="form-control"
            id="title"
            value={title}
            onChange={this.handleChange}
          />
        </div>
        <button type="submit" className="btn btn-success btn-lg">
          SAVE
        </button>
      </form>
    );
  }
}

const Form = connect(null, mapDispatchToProps)(ConnectedForm);

export default Form;
```



**mapDispatchToProps connects Redux actions to React props**

```js
// ...
  handleSubmit(event) {
    event.preventDefault();
    const { title } = this.state;
    const id = uuidv1();
    this.props.addArticle({ title, id }); // Relevant Redux part!!
// ...
  }
// ...
```

This way a connected component is able to dispatch actions.



Update App to include the Form component:

```js
import React from "react";
import List from "./List";
import Form from "./Form";

const App = () => (
  <div className="row mt-5">
    <div className="col-md-4 offset-md-1">
      <h2>Articles</h2>
      <List />
    </div>
    <div className="col-md-4 offset-md-1">
      <h2>Add a new article</h2>
      <Form />
    </div>
  </div>
);

export default App;
```

Install uuid with:

```bash
npm i uuid --save-dev
```

Now run webpack (or Parcel) with:

```bash
npm start
```

and head over to http://localhost:8080