# React Lesson

Stefan Schwartze

---

## About React

Note: (about 45 to 60 minutes)
---

### What is React?
Just a SIMPLE view library
Note: 
* No router, no state management, no HTTP client etc  (keeps it small, no framework)
* Difference compared to Angular etc.
---

### Basic principles of React
----
#### JSX
----
Looks like: HTML with Javascript 
Is: Javascript used for templates
How to use?

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
#### React DOM
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
* Universal Javascript (one codebase for all)
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