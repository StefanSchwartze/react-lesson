# React Lesson

Stefan Schwartze

---

## About React

Note: (about 45 to 60 minutes)
---

### What is React?
* Library for creating interactive UIs
* Created by Facebook in 2013
* Goal: Splitting UI in different components
Note:
* Very popular because of simplicity (at that time compared to Angular)
* No router, no state management, no HTTP client etc  (keeps it small, no framework)
* Difference compared to Angular etc.
* TODO: Table with React, Angular etc.
---

### Basic principles of React
----
#### JSX (JavaScript XML)
----
``` javascript
const element = <h1>Hello, world!</h1>;
```
* **Looks like**: HTML mixed with JavaScript 

* **Is**: No _string_, no _HTML_. Just JavaScript used for templates
Note:
* React works without JSX, but nobody would ever do this
----
##### Inserting strings
``` javascript
const text = 'Hello, world!';
const element = <h1>{text}</h1>;
```
----
##### Inserting attributes
```
const uniqueId = 'unique';
const element = <h1 id={uniqueId}>Hello, world!</h1>;
```  
<br/>
###### Special cases
* `class` -> `className`
* ` for ` -> `htmlFor`
* Always in **camelCase**: ` tabindex ` -> `tabIndex`
Note: 
* Example pen by React: https://codepen.io/pen?&editors=0010
----

###### JSX ~~required~~
<br/>
``` javascript
const element = <h1 className="greeting">Hello, world!</h1>;
```
_equals to_
``` javascript
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```
Note:
* JSX is compiled to React.createElement calls
----

#### Output:

``` javascript
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```
----

#### React DOM
_attaching elements to the DOM_

**HTML**
```
<div id="root"></div>
```

**JavaScript**
``` javascript
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```
----
##### [Example for re-rendering](https://reactjs.org/redirect-to-codepen/rendering-elements/update-rendered-element)

Note:
* Show updates in DevTools
---
### Components
----

#### What are components? 
* Encapsulated, independent
* Can be nested to each other

![alt Example structure of a blogging platform](https://cdn-images-1.medium.com/max/1600/0*4p1fVzn0x6rh6kL9.png)
----
TODO: Image of component tree!

#### Creating a component

```javascript
const Welcome = (props) => { // Functional
  return <h1>Hello, {props.name}</h1>;
}
```
_equals to_
```javascript
class Welcome extends React.Component { // Class / Stateful
  render() {
    return <h1>Hello, {props.name}</h1>;
  }
}
```
Note:
From React's point of view equal
----
### How to render a component
<br/>
**HTML element**:
```javascript
const element = <div />;
```

**Component**:
```javascript
const element = <Welcome name="Sara" />;
```
----

#### [Example](https://reactjs.org/redirect-to-codepen/components-and-props/rendering-a-component)
```javascript
const Welcome = (props) => {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
Note:
* We call ReactDOM.render() with the <Welcome name="Sara" /> element.
* React calls the Welcome component with {name: 'Sara'} as the props.
* Our Welcome component returns a <h1>Hello, Sara</h1> element as the result.
* React DOM efficiently updates the DOM to match <h1>Hello, Sara</h1>.
----

#### Functional vs stateless vs stateful components
* **Functional**: simple JavaScript functions
* **Stateless**: only render props received from outside
* **Stateful**: manage internal state
----

#### Props 
* Retrieving information from parent components
* Read-only
----
##### Prop Types

```javascript
import PropTypes from 'prop-types'; 
import React from 'react';
class BlogPostExcerpt extends React.Component { 
  render() { 
    return ( 
      <div> 
        <h1>{this.props.title}</h1> 
        <p>{this.props.description}</p> 
      </div> 
    ) 
  } 
} 
BlogPostExcerpt.propTypes = { 
  title: PropTypes.string, 
  description: PropTypes.string 
}; 
export default BlogPostExcerpt;
```
Note:
* TODO: Codepen example with default props etc.
----
**Possible types**

* `PropTypes.array`
* `PropTypes.bool`
* `PropTypes.func`
* `PropTypes.number`
* `PropTypes.object`
* `PropTypes.string`
* `PropTypes.symbol`
* `PropTypes.oneOf(['Test1', 'Test2'])`
* `PropTypes.instanceOf(Something)`
* `PropTypes.arrayOf(PropTypes.string)`
----

* Requiring properties
```javascript
PropTypes.arrayOf(PropTypes.string).isRequired
PropTypes.string.isRequired
```

* Default values for props
```javascript
BlogPostExcerpt.propTypes = { 
  title: PropTypes.string, 
  description: PropTypes.string 
} 
BlogPostExcerpt.defaultProps = { 
  title: '', 
  description: '' 
}
```
----

##### Passing props
```javascript
const desc = 'A description' 
//... 
<BlogPostExcerpt title="A blog post" description={desc} />
```
----
##### Children

```javascript
<Welcome name="Max"> 
  <p>I'am a child!</p>
</Welcome>
```
Can be used with `this.props.children`:
```javascript
class Welcome extends Component { 
  render() { 
    return (
      <div>
        <h1>{this.props.name}</h1>
        <ul>{this.props.children}</ul>
      </div> 
    ) 
  } 
} 
```
Note:
* Doesn't work for functional components because class is missing
----
#### State
* State stays internal
* Can be modified
----
##### Default state
```javascript
class BlogPostExcerpt extends Component {
  constructor(props) { 
    super(props);
    this.state = { clicked: false };
  } 
  render() { 
    return (
      <div> 
        <h1>Title</h1> 
        <p>Description</p> 
        <p>Clicked: {this.state.clicked}</p> 
      </div> 
    ) 
  } 
}
```
Note:
* constructor initializes component
* super() is required for calling React.Components constructor
----

##### Mutating the state

* State should **never** be mutated directly:
```javascript
this.state.clicked = true;
```

* Instead use:
```javascript
this.setState({ clicked: true});
```

-> _Important to inform React about State changes_

----
##### Converting a functional to a stateful component

```javascript
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```
[Codepen](http://codepen.io/gaearon/pen/dpdoYR?editors=0010)
----

#### Unidirectional data flow

![alt Data flow in React](https://i2.wp.com/pbs.twimg.com/media/DInWVnyVAAEAtP3.jpg?w=640&ssl=1)

Note:
* Props get passed from top to bottom
* State stays internally per component
* TODO: nice graphic!
----
##### Moving state up in the components tree

```javascript
class Converter extends React.Component { 
  constructor(props) { 
    super(props)
    this.state = { currency: '€' } 
  } 
  render() { 
    return ( 
      <div> 
        <Display currency={this.state.currency} />
        <CurrencySwitcher currency={this.state.currency} />
      </div> 
    ) 
  } 
}
```
Note:
* State will from now be maintained in parent component
----
##### Mutating functions in props to talk to parent
```javascript
render() { 
  return (
    <CurrencySwitcher 
      currency={this.state.currency} 
      handleChangeCurrency={this.handleChangeCurrency} 
    /> 
  )
}
```
```javascript
const CurrencySwitcher = (props) => { 
  return ( 
    <button onClick={props.handleChangeCurrency}> 
      Current currency is {props.currency}. Change it! 
    </button> 
  ) 
}
```
[Codepen 1](https://codepen.io/anon/pen/XEpoqO?editors=0010)
[Codepen 2](https://codepen.io/gaearon/pen/WZpxpz?editors=0010)

Note:
* handlers can be compared to Angulars Output
----

#### Event handlers
---

### JSX: Conditional rendering and lists
----

#### Conditions
```javascript
function Greeting(props) {
  if (props.isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```
Note: 
* Angular uses specific keywords like *ngIf, JSX just uses plain JavaScript
----

### Lists
```javascript
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
[Codepen](https://codepen.io/gaearon/pen/jrXYRR?editors=0011)
Note:
* Show also with TodoList
----

### Examples comparing Angular vs. React
----

#### Simple looping

Angular
```
<ul *ngFor="let item of items">
  <li>{{item.text}}</li>
</ul>
```

React
```javascript
<ul>
  {items.map(item => <li>{item.text}</li>)}
</ul>
```
----

#### Index

Angular
```
<ul *ngFor="let item of items; let i = index">
  <li>{{i}} {{item.text}}</li>
</ul>
```

React
```javascript
<ul>
  {items.map((item, i) => 
    <li>{i} {item.text}</li>
  )}
</ul>
```
----

#### Keys / trackBy

Angular
```
<ul *ngFor="let item of items; trackBy: trackByFn">
  <li>{{item.text}}</li>
</ul>
...
trackByFn(index, item) {
  return index;
}
```

React
```javascript
<ul>
  {items.map(item => 
    <li key={item.id}>{item.text}</li>
  )}
</ul>
```
----

#### Filtering vs. Pipes

Angular
```
<ul *ngFor="let item of items | filterDone">
  <li>{{item.text}}</li>
</ul>

@Pipe({
  name:"filterDone"
})
class FilterDone {
  transform(items: string): Array<Todo> {
    return items.filter({done} => done);
  }
}
```
----

React
```javascript
<ul>
  {items.filter({done} => done).map(item => 
    <li key={item.id}>{item.text}</li>
  )}
</ul>
```

[Codepen](https://codepen.io/gaearon/pen/NRZYGN?editors=0010)
----

* Always use keys to avoid re-rendering
* Use plain JS functions for conditions and loops

Note:
Use case | Angular | React
--- | --- | ---
Conditions | *ngIf / else / then | a ? b : c (pure JS)
Loops | *ngFor | .map, .forEach ...
Indexing | trackByFn | key = ...
Reduce, filter | Custom func. | JS
---

### React lifecycle methods
----
Use cases for every method
Note: 
* componentWillMount()
* componentDidMount()
  * API request
* shouldComponentUpdate()
  * Only update if specific condition has changed
* componentReceivedProps()
---

### React Example application consuming a API (DEMO)
Note: 
Show props and state changes in React developer tools
---

### Advantages of React:
* Small / Simple / Modular
* Very comfortable JSX syntax (supporting full JS usage)
  - No custom keywords required like *ngFor, *ngIf, pipes | v-if, v-for, just plain JS!
* Virtual DOM 
  - What and why?
* Universal JavaScript (one codebase for all)
  - allows server side rendering
  - doesn’t require client to enable JS
---





BREAK
————————————————————————————————

PART 2: Using React (about 20 - 30 minutes (+ evtl. 30 minutes) ) —> talk + discussions (+ eventually Hands On with coding)

Short recap

Styling
Global
Component-based
Inline

Routing using React-Router (v4)
Note:
* Be careful on Stackoverflow with old versions

Basic vs dumb vs smart components

(Coding a React application / Hands On)

BREAK
————————————————————————————————

PART 3: FLUX (about 30 minutes) —> talk + discussions (OPTIONAL)

Problem: sharing state between components

What is FLUX?
Maintaining application state
Just a pattern, no library

Basic principles of FLUX
Store
Actions
Dispatchers

Example using Redux



## Sources

* React docs: https://reactjs.org/docs
* Flavio Copes: https://medium.freecodecamp.org/the-beginners-guide-to-react-9be65f50a55c
* 