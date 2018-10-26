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



