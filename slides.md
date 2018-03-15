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
* Very popular because of simplicity
* No router, no state management, no HTTP client etc  (keeps it small, no framework)
* Difference compared to Angular etc.
---

### Basic principles of React
----
#### JSX (JavaScript XML)
----
```
const element = <h1>Hello, world!</h1>;
```
* **Looks like**: HTML mixed with JavaScript 

* **Is**: No _string_, no _HTML_. Just JavaScript used for templates
Note:
* React works without JSX, but nobody would ever do this
----
##### Inserting strings
```
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
----

----
#### Components
----
What are components? Encapsulated, independent
can be nested to each other
----
Example: Page, separated in single components
---

### How to render a component
----
#### Static vs stateful components
----
#### Props 
for getting information from parent components
----
Prop Types
----
Passing props
----
Children
#### State
state stays internal
----
Default state, Accessing, mutating state
----
Example that shows how state + props work (DEMO)
----
Unidirectional data flow (sharing state between components)
----
Moving state up in the components tree
----
Event handlers
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
    * 
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

Routing using React-Router

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