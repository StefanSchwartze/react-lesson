# React Lesson

Stefan Schwartze

---

## Before we start
* All code examples are written in **ES6**
* Code applies to React **v16**
* Don't hesitate to directly ask for questions
---

## About React
* **Library** for creating interactive UIs
* Created by Facebook in 2013
* Goal: Splitting UI in different components
Note:
* Very popular because of simplicity (at that time compared to Angular)
* No router, no state management, no HTTP client etc  (keeps it small, no framework)
* Difference compared to Angular etc.
* TODO: Table with React, Angular etc.
---

## Basic principles of React
----
### JSX (JavaScript XML)
----
```jsx
const element = <h1>Hello, world!</h1>;
```
* **Looks like**: HTML mixed with JavaScript 

* **Is**: No _string_, no _HTML_. Just JavaScript used for templates
Note:
* React works without JSX, but nobody would ever do this
* Usually compiled within transpilers like Babel
----
#### Inserting strings
```jsx
const text = 'Hello, world!';
const element = <h1>{text}</h1>;
```
----
#### Inserting attributes
```jsx
const uniqueId = 'unique';
const element = <h1 id={uniqueId}>Hello, world!</h1>;
```  
<br/>
##### Special cases
* `class` -> `className`
* ` for ` -> `htmlFor`
* Always in **camelCase**: ` tabindex ` -> `tabIndex`
Note: 
* Example pen by React: https://codepen.io/pen?&editors=0010
----

##### JSX ~~required~~
<br/>
```jsx
const element = <h1 className="greeting">Hello, world!</h1>;
```
_equals to_
```jsx
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```
Note:
* JSX is compiled to React.createElement calls
----

### Output:

```jsx
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```
----

### React DOM
_attaching elements to the DOM_

**HTML**
```markup
<div id="root"></div>
```

**JavaScript**
```jsx
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```
----
#### [Example for re-rendering](https://reactjs.org/redirect-to-codepen/rendering-elements/update-rendered-element)

Note:
* Show updates in DevTools
---
## Components
----

### What are components? 
* Encapsulated, independent
* Can be nested to each other

![alt Example structure of a blogging platform](https://cdn-images-1.medium.com/max/1600/0*4p1fVzn0x6rh6kL9.png)
----
TODO: Image of component tree!

### Creating a component

```jsx
const Welcome = (props) => { // Functional
  return <h1>Hello, {props.name}</h1>;
}
```
_equals to_
```jsx
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
```jsx
const element = <div />;
```

**Component**:
```jsx
const element = <Welcome name="Sara" />;
```
Note: 
* Components can be recognized by capitalizing
----

### [Example](https://reactjs.org/redirect-to-codepen/components-and-props/rendering-a-component)
```jsx
const Welcome = props => {
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

### Functional vs stateless vs stateful components
* **Functional**: simple JavaScript functions
* **Stateless**: only render props received from outside
* **Stateful**: manage internal state
----

### Props 
* Retrieving information from parent components
* Read-only
----

#### Passing props
```jsx
const desc = 'A description' 
//... 
<BlogPostExcerpt title="A blog post" description={desc} />
```
----
#### Prop Types

```jsx
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

Note:
PropTypes are being removed in production build
----

* Requiring properties
```jsx
PropTypes.arrayOf(PropTypes.string).isRequired
PropTypes.string.isRequired
```

* Default values for props
```jsx
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

#### Children

```jsx
<Welcome name="Max"> 
  <p>I'am a child!</p>
</Welcome>
```
Can be used with `this.props.children`:
```jsx
class Welcome extends Component { 
  render() { 
    return (
      <div>
        <h1>{this.props.name}</h1>
        <div>{this.props.children}</div>
      </div> 
    ) 
  } 
} 
```
Note:
* Doesn't work for functional components because component class is missing
----
### State
* State stays internal
* Can only be modified by component (not from outside)
----
#### Default state
```jsx
class BlogPostExcerpt extends React.Component {
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

#### Mutating the state

* State should **never** be mutated directly:
```jsx
this.state.clicked = true;
```

* Instead use:
```jsx
this.setState({ clicked: true});
```

-> _Important to inform React about State changes_
Note:
* setState is asynchronous, callback can be used as 2nd parameter
----
#### Converting a functional to a stateful component

```jsx
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

### Unidirectional data flow

![alt Data flow in React](https://i2.wp.com/pbs.twimg.com/media/DInWVnyVAAEAtP3.jpg?w=640&ssl=1)

Note:
* Props get passed from top to bottom
* State stays internally per component
* TODO: nice graphic!
----
#### Moving state up in the components tree

```jsx
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
#### Mutating functions in props to talk to parent
```jsx
render() { 
  return (
    <CurrencySwitcher 
      currency={this.state.currency} 
      handleChangeCurrency={this.handleChangeCurrency} 
    /> 
  )
}
```
```jsx
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

### Events

* **HTML**
```html
<button onclick="handleClick()"> ... </button>
```

* **React**
```jsx
<button onClick={this.handleClick}> ... </button> 
```

* React always uses __camelCase__:
	* onsubmit -> onSubmit etc.
* **Don't** use any event listeners
* Huge list of events available in [React Docs](https://reactjs.org/docs/events.html#clipboard-events)
----

### Handling events

Best practice: **Handlers**
```jsx
class Button extends React.Component { 
	handleClick(event) => { 
		// do something
	}
	render() {
		return (
      <button onClick={this.handleClick}>Click me</button>
		);
	}
}
```
* All handlers receive events according to [W3C UI Events spec](https://www.w3.org/TR/DOM-Level-3-Events/).
----

#### Methods not bound by default
```jsx
...
handleClick() => { 
	console.log(this) // undefined
}
...
```

Bind `this` in constructor:
```jsx
constructor(props) {
	super(props);
	// This binding is necessary to make `this` work in the callback
	this.handleClick = this.handleClick.bind(this);
}
```
----

#### New experimental syntax (ES7)
```jsx
// This syntax ensures `this` is bound within handleClick.
// Warning: this is *experimental* syntax.
handleClick = () => {
	console.log('this is:', this);
}
```
Note:
* Can be enabled as Babel plugin
* Enabled in `create-react-app` by default
----
#### Preventing default behaviour
```jsx
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return <a href="#" onClick={handleClick}>Click me</a>;
}
```
----

## JSX: Conditional rendering and lists
----

### Conditions
```jsx
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
```jsx
function NumberList({ numbers }) {
  const listItems = numbers.map(number =>
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
* TODO: Show also with TodoList
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
```jsx
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
```jsx
<ul>
  {items.map((item, i) => 
    <li>{i} {item.text}</li>
  )}
</ul>
```
----

#### Keys vs. trackBy

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
```jsx
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
  transform(items: Array<Todo>): Array<Todo> {
    return items.filter({done} => done);
  }
}
```
----

React
```jsx
<ul>
  {items.filter({done} => done).map(item => 
    <li key={item.id}>{item.text}</li>
  )}
</ul>
```

[Codepen](https://codepen.io/gaearon/pen/NRZYGN?editors=0010)
----

* **Always** use keys to avoid re-rendering
* **Don't** use index: can cause problems with sorting; is used by default
* **Use** plain JS functions for conditions and loops

Note:
Use case | Angular | React
--- | --- | ---
Conditions | *ngIf / else / then | a ? b : c (pure JS)
Loops | *ngFor | .map, .forEach ...
Indexing | trackByFn | key = ...
Reduce, filter | Custom func. | JS
---

## React lifecycle methods
![alt Lifecycle methods](https://cdn-images-1.medium.com/max/2000/1*XcGM-8E_hGl4fpAr9wJIsA.png)
----

### componentWillMount()

* Directly called after constructor
* Can call **setState**: Yes. But **don't** do that. Use default state instead.

Note:
* Calling setState will not trigger extra rendering
* React recommends to avoid any side-effects or subscriptions in this method
* Use didMount instead
* Only method called on the server
* Can be compared to onInit in Angular
----

### componentDidMount()

* Component is mounted, ready to use
* Perfect to setup subscriptions or AJAX requests
* Used for initalizations that require DOM
* Can call **setState**: **Yes**. Will cause an extra rendering.

Note:
* e.g. modals, tooltips, positioning of elements
* draw on a <canvas> element that you just rendered
* initialize a masonry grid layout from a collection of elements
* add event listeners
* Can be compared to AfterViewInit in Angular
----

### componentWillReceiveProps()

* Called when new data from parent component arrives
* Used for state updates in response to prop changes
* Can call **setState**: **Yes**.

Note:
* has access to old and new props + state
* Also is called when same props are passed
* Example: drawing `canvas` circle when perecentage changed
* Can be compared to onChanges in Angular
----

### shouldComponentUpdate()

* Called before re-rendering when state or props changed
* Used for performance improvements / preventing re-rendering
* Returning **`false`** prevents updates
* Can call **setState**: **No**.

Note:
* defaults to true
* stop updating when nonsense in props has changed
* doing deep nested comparisons is expensive and not recommended
----

#### Example
```jsx
shouldComponentUpdate(nextProps, nextState) {
	return this.props.title !== nextProps.title || 
				this.state.loading !== nextState.loading;
}
```

Note:
* only use when having performance problems
* React.PureComponent implements method with a shallow compare
* Comparable to ngDoCheck in Angular
----

### componentWillUpdate()

* Immediately called *before* rendering
* Can call **setState**: **No**.
----

### componentDidUpdate()

* Immediately called *after* rendering
* Can be used for new network requests or interacting with DOM
* Can call **setState**: **Yes**.
----

### componentWillUnmount()

* Invoked before removing component
* Used for last cleanups
```jsx
  componentWillUnmount() {
    window.removeEventListener('resize', this.resizeListener);
  }
```
* Can call **setState**: **No**.

Note:
* Can be compared to onDestroy in Angular
* Example: (https://gist.github.com/treyhuffine/8728583fd2985bd0cab1dfa403ba49ec#file-react-16-error-boundary-example-jsx)
----

### componentDidCatch()

* Prevents crashing whole application
* Allows to display some fallback UI

Note:
* Only available since React 16
* TODO: example code
----

### Conclusion

* Ideally not used
* In the real world: shouldComponentUpdate and componentDidMount mostly used
* Can be used for performance improvements
----

![alt Lifecycle methods](https://cdn-images-1.medium.com/max/1600/1*u8hTumGAPQMYZIvfgQMfPA.jpeg)
---

## Basic vs dumb vs smart
Note:
* TODO
---

## High-order components (Optional)
Note:
* TODO
---

## Styling components
Note:
* There are tons of plugins and ways how to style, here are some of the most common ones
----

### One stylesheet
* Just reference a global stylesheet as usual
* **Not** recommended. Only use for very small projects (confusing / component context gets lost)
----

### Stylesheet per component
* Just import plain stylesheet in your component:
  ```jsx
  import './style.css';
  
  ...

  render() {
    <div className="box">Just a styled box.</div>
  }
  ```
* **Good**: clear component context
* **Bad**: still global styles
----

### Inline
* Javascript objects
* *camelCase* styles
```jsx
const boxStyle = {
    width: '100%',
    boxShadow: '0 2px 4px black'
}
render() {
    <div style={boxStyle}>Just a styled box.</div>
}
```
* **Good**: local styles
* **Bad**: bad performance, ugly code, hard to maintain

Note:
* Inline styles only support a subset of CSS. Putting your styles inline into the DOM means you cannot use pseudo selectors, media queries, keyframes etc. (because the browser doesn’t recognise those things) | Max Stoiber 2016
----

### CSS Modules ([Demo](https://css-modules.github.io/webpack-demo/))
* Local styles by default
```jsx
  import styles from './ScopedSelectors.css';
  import React from 'react';

  export default class ScopedSelectors extends React.Component {

    render() {
      return (
        <div className={ styles.root }>
          <p className={ styles.text }>Scoped Selectors</p>
        </div>
      );
    }

  };
```
* Global is optional
----

### CSS-in-JS
* Mix of CSS Modules and Inline styles
```jsx
  import { StyleSheet, css } from 'aphrodite';

  const styles = StyleSheet.create({
    button: {
      backgroundColor: 'palevioletred',
      color: 'papayawhip',
    },
  });

  <button className={css(styles.button)} />
```
* Output can be compared to CSS Modules
----

### Keep in mind
* *React* has no opinion how to style components
* Always use method that fits best to your project
* Only load styles you need
* Keep your styles maintainable
* DRY

Note:
* In most cases just importing stylesheets per component (in combination with preprocessors) is totally sufficent
---

## Routing using React-Router
Note:
* Feel free to use your own implementation
* React-Router is most popular
* Be careful on Stackoverflow with old versions (<v4)
----

### Installation

React Router exposes **3** packages:
* `react-router`, `react-router-dom`, `react-router-native`
```shell
yarn add react-router-dom
```
----

### Rendering a Router
```jsx
import { BrowserRouter } from 'react-router-dom'
ReactDOM.render((
  <BrowserRouter>
    <App />
  </BrowserRouter>
), document.getElementById('root'))

const App = () => (
  <div>
    <Header />
    <Main />
  </div>
)
```
* **One** child allowed only
* **`MemoryRouter`** for serverside usage
----

### Routes
```jsx
const Main = () => (
  <main>
    <Route path="/" exact component={Home} />
    <Route path="/posts" component={Posts} />
  </main>
)
```
* No central route file
* Routing can be nested in every Router's child component
* `exact` - will be default in v5
----

### Exclusive routing
```jsx
import { Switch, Route } from 'react-router-dom'
const Main = () => (
  <main>
    <Switch>
      <Route path="/" exact component={HomePage} />
      <Route path="/posts" component={PostsPage} />
      <Redirect to="/" />
    </Switch>
  </main>
)
```
Note:
* Redirect can be used for 404 pages e.g.
----

### Nested routes
```jsx
import { Switch, Route } from 'react-router-dom'
const PostsPage = props => (
    <Switch>
      <Route path="/posts" exact component={PostsList} />
      <Route path="/posts/:postId" component={PostDetail} />
    </Switch>
)
```
----

### Match
`match` object is provided in props

* **url** — the matched part of the current location’s pathname
* **path** — the route’s path (without any params etc.)
* **isExact** — path === pathname
* **params** — an object containing values from the pathname that were captured by path-to-regexp

Note:
* Difference between url and path: url contains matched string, path contains params
----

### Using path from match
```jsx
import { Switch, Route } from 'react-router-dom'
const PostsPage = props => (
    <Switch>
      <Route path={props.match.path} exact component={PostsList} />
      <Route path={`${props.match.path}/:postId`} component={PostDetail} />
    </Switch>
)
```
----

### Using params from match
```jsx
// an API that returns a post object
import postAPI from './postAPI'
const Post = props => {
  const post = postAPI.get(props.match.params.postId);
  if (!post) {
    return <div>Sorry, but the post was not found</div>
  }
  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
    </div>
)
```
----

### Navigating with Links
```jsx
import { Link } from 'react-router-dom'
const Header = () => (
  <header>
    <nav>
      <ul>
        <li><Link to='/'>Home</Link></li>
        <li><Link to='/posts'>Posts</Link></li>
      </ul>
    </nav>
  </header>
)
```
----

### Add sugar with NavLink
```jsx
import { NavLink } from 'react-router-dom'
const Header = () => (
  <header>
    <nav>
      <ul>
        <li><NavLink to='/' exact activeClassName="active">Home</NavLink></li>
        <li><NavLink to='/posts' activeClassName="active">Posts</NavLink></li>
      </ul>
    </nav>
  </header>
)
```
----

### History

* Generated by the router
* `history.location` object contains all information about current location
```jsx
// a basic location object
{ 
    pathname: '/', 
    search: '',
    hash: '',
    key: 'abc123'
    state: {
      modalOpen: true
    } 
}
```
Note:
* A React Router component that does not have a router as one of its ancestors will fail to work.
----

### Navigating programmatically
* Push
```javascript
history.push({ pathname: '/new-place' })
```
* Replace (good for redirects)
```javascript
history.replace({ pathname: '/go-here-instead' })
```
* Let's go everywhere:
```javascript
history.go(-3)
history.goBack()
```
Note:
* `.push`and `.replace` accept same object like `location` provides
----

### Accessing match / history / location

* Route component
```jsx
import { Switch, Route } from 'react-router-dom'
const PostsPage = ({ match, location, history }) => (
    <Switch>
      <Route path="/posts" exact component={PostsList} />
      <Route path="/posts/:postId" component={PostDetail} />
    </Switch>
)
```
* Route render
```jsx
import { Route } from 'react-router-dom'
const Button = () => (
  <Route render={({ match, location, history }) => (
    <button
      type='button'
      onClick={() => { history.push('/new-location') }}
    >
      Click Me!
    </button>
  )} />
)
```
----

* Route children
```jsx
import { Route } from 'react-router-dom'
<Route path='/posts' children={({ match, location, history }) => (
  props.match ? <PostsPage {...props}/> : <EmptyPage {...props}/>
)}/>
```
* withRouter
```jsx
import { withRouter } from 'react-router-dom'
const Button = withRouter(({ match, location, history }) => (
  <button
    type='button'
    onClick={() => { history.push('/new-location') }}
  >
    Click Me!
  </button>
))
```
Note:
* Best: use a decorator with @withRouter (ES7)
* TODO: Codepen example!
----

### Recap
* Everything is a component / decentralized routing
* `<BrowserRouter>`, `<Route>` and `<Link>` are mostly used
* Programmatically navigate using `history.push()` etc.

Note:
* Layouts should keep that decentralized approach in mind
---

## Advantages of React:
* Small / Simple / Modular
* Very comfortable JSX syntax (supporting full JS usage)
  - No custom keywords required like *ngFor, *ngIf, pipes | v-if, v-for, just plain JS!
* Virtual DOM 
  - What and why?
* Universal JavaScript (one codebase for all)
  - allows server side rendering
  - doesn’t require client to enable JS
* Great community that contributes to the project
* Tons of free open-source components
---

## State management
----
### Problem: sharing state between components
----

### Using stores for UI state
* Storing the state centralized
* Subscribing to changes
---

## Flux
----

### What is Flux?
* Just a pattern, no library
* Maintaining application state
----

### Principles of Flux
  - Actions
  - Dispatcher
  - Stores
  - (View)
----

### How does it work?
![alt Flux concept simple](https://github.com/facebook/flux/raw/master/examples/flux-concepts/flux-simple-f8-diagram-with-client-action-1300w.png)
*One-directional data flow*
Note:
General:
* Action describes something that possibly changes some state
* Dispatcher is a singleton that receives all actions and dispatches them to all registered stores
* Stores save the state
* View is React frontend and subsribes to Stores (possibly in componentDidMount)

Data Flow:
* View / user interaction triggers an action and sends it to dispatcher
* Dispatchers sends the action to all stores that have been registered before
* Store(s) then know what to do with data (and if to change the state)
  * Must only be mutated by respnding to an action
* All stores that have changed must emit an change event
* View gets change event and re-renders with new data
![alt Flux concept extended](https://github.com/facebook/flux/raw/master/docs/img/flux-diagram-white-background.png)
---

![alt Redux](https://kyleshevlin.com/wp-content/uploads/2016/11/redux_logo_2.png)
----
## Redux
- Inspired by Flux
- **One** store replicating whole state tree
- Does **not** mutate the state, always creates a new one 
- Allows time-traveling
----

### Difference to Flux
![alt Flux vs. Redux](https://i.stack.imgur.com/HbS0b.jpg)
----

### Redux explained in Cartoon
*Credits: Lin Clark*
----

#### Action Creators
![alt Action Creators](https://cdn-images-1.medium.com/max/1200/1*Uljrrh4Z7UiUwk8AjUO9PA.png)
----

#### *The* Store
![alt *The* Store](https://cdn-images-1.medium.com/max/1200/1*Gztc7THzxzOgJmGvJ95IQA.png)
----

#### Reducers
![alt Reducers](https://cdn-images-1.medium.com/max/1200/1*Vocy_6Gl9PbFlCIJsE9r3A.png)
----

#### The View(s)
![alt The View(s)](https://cdn-images-1.medium.com/max/1200/1*TgCkFcjlD9SxSrMvVX3DrA.png)
----

#### The view layer binding
![alt The view layer binding](https://cdn-images-1.medium.com/max/1200/1*D1RcVrMV2rp6AH9hk5xZ8g.png)
----

#### Root component
![alt Root component](https://cdn-images-1.medium.com/max/1200/1*JXPeiNP-it60-QYKb-p2eQ.png)
----

### Setup
#### 1. Get the store ready
![alt 1. Get the store ready](https://cdn-images-1.medium.com/max/1600/1*8_fU31-jNQnQ0dp-wplm5w.png)
Note:
* Get the store ready. The root component creates the store, telling it what root reducer to use, using createStore(). This root reducer already has a team of reducers which report to it. It assembled that team of reducers using combineReducers().
----

#### 2. Set up the communication
![alt 2. Set up the communication](https://cdn-images-1.medium.com/max/1600/1*NYMutQLW8TcEgbO8VNeqHA.png)
Note:
* Set up the communication between the store and the components. The root component wraps its subcomponents with the provider component and makes the connection between the store and the provider.
* The Provider creates what’s basically a network to update the components. The smart components connect to network using connect(). This ensures they’ll get state updates.
----

#### 3. Prepare action callbacks
![alt 3. Prepare action callbacks](https://cdn-images-1.medium.com/max/1600/1*aVoD3gGddKUy3VCxwylthQ.png)
Note:
* Prepare the action callbacks. To make it easier for dumb components to work with actions, the smart components can setup action callbacks by using bindActionCreators(). This way, they can just pass down a callback to the dumb component. The action will be automatically dispatched after it is formatted.
----

### Redux in action
#### Incoming action
![alt Incoming action](https://cdn-images-1.medium.com/max/1600/1*GNDs7SY53lEhp7mX8V25lw.png)
Note:
* Some user interaction causes an action
----

#### 1. View to action creator
![alt View to action creator](https://cdn-images-1.medium.com/max/1600/1*p4EkWE_8upZ97Z0IapKDcQ.png)
Note:
* The view requests an action. The action creator formats it and returns it.
----

#### 2. Dispatching
![alt Dispatching](https://cdn-images-1.medium.com/max/1600/1*zmFp3bmDq7b6Bvlo8Ineag.png)
Note:
* The action is either dispatched automatically (if bindActionCreators() was used in setup), or the view dispatches the action.
----

#### 3. Store to reducer
![alt Store to reducer](https://cdn-images-1.medium.com/max/1600/1*zrsSoAAyf4pqTMHiA6P8Ww.png)
Note:
* The store receives the action. It sends the current state tree and the action to the root reducer.
----

#### 4. Root reducer to subreducers
![alt Root reducer to subreducers](https://cdn-images-1.medium.com/max/1600/1*-S_dYe6BoQBgwSRpF7Hriw.png)
Note:
* The root reducer cuts apart the state tree into slices. 
* Then it passes each slice to the subreducer that knows how to deal with it.
----

#### 5. Creating new modified state
![alt Creating new modified state](https://cdn-images-1.medium.com/max/1600/1*_R-rGNfKr2Xu2FlXNZNPJg.png)
Note:
* The subreducer copies the slice and makes changes to the copy. It returns the copy of the slice to the root reducer.
----

#### 6. Pasting all together
![alt Pasting all together](https://cdn-images-1.medium.com/max/1600/1*bUMekI8QlEfFxSBCuVuIkw.png)
Note:
* Once all of the subreducers have returned their slice copies, the root reducer pastes all of them together to form the whole updated state tree, which it returns to the store.
* The store replaces the old state tree with the new one.
----

#### 7. Store emits change
![alt Store emits change](https://cdn-images-1.medium.com/max/1600/1*x6vBvUlFJktJqty56jr0QQ.png)
Note:
* The store tells the view layer binding that there’s new state.
----

#### 8. State request
![alt State request](https://cdn-images-1.medium.com/max/1600/1*qGatznV4QujuxGe49YfX5A.png)
Note:
* The view layer binding asks the store to send over the new state.
----

#### 9. Re-rendering
![alt Re-rendering](https://cdn-images-1.medium.com/max/1600/1*Je2mow8mjYLngXreGGlIEg.png)
Note:
* The view layer binding triggers a rerender.
----

### Wrapping it all up
- Less magic, more boilerplate
- Pure / functional programming
- Saving state allows TT and HR
---

 ![alt MobX](https://seeklogo.com/images/M/mobx-logo-0C59CBBAD9-seeklogo.com.png)
## MobX
- Always mutates the state
- Really easy setup
---

## MobX vs. Redux
MobX | Redux
--- | ---
Mutable | Immutable
Multiple stores | One store
Object-oriented | Functional
Easy getting started | Much boilerplate code
No rollback | Timetravel by default
----

### Which one is better?
![alt Redux vs. MobX](https://image.slidesharecdn.com/makingreactpartofsomethinggreater-161031165748/95/making-react-part-of-something-greater-40-638.jpg?cb=1477933169)
----

### Do I need it?
* Depends. Start with setState.
* When bubbling along tree becomes **too** complicated, go!
* Keep in mind: state management adds complexity!
Note:
---

### Hands on your keyboard
* Be sure **Node >= 6** is installed
* Get started:
```bash
  npx create-react-app my-app
  cd my-app
  npm start
```
* Use React developer tools for debugging
----

## React developer tools
Note: 
* Show props and state changes in React developer tools
* TODO: Example pages
---

### Task I


Note:
PART 2: Using React (about 20 - 30 minutes (+ evtl. 30 minutes) ) —> talk + discussions (+ eventually Hands On with coding)
PART 3: FLUX (about 30 minutes) —> talk + discussions (OPTIONAL)

## Sources

* React docs: https://reactjs.org/docs
* Flavio Copes: https://medium.freecodecamp.org/the-beginners-guide-to-react-9be65f50a55c
* Scott Domes: https://engineering.musefind.com/react-lifecycle-methods-how-and-when-to-use-them-2111a1b692b1
* Trey Huffine: https://levelup.gitconnected.com/componentdidmakesense-react-lifecycle-explanation-393dcb19e459
* Max Stoiber: https://mxstbr.blog/2016/11/inline-styles-vs-css-in-js/
* Paul Sherman: https://medium.com/@pshrmn/a-simple-react-router-v4-tutorial-7f23ff27adf
* Brad Westfall: https://css-tricks.com/react-router-4/
* Facebook: https://github.com/facebook/flux/tree/master/examples/flux-concepts
* Lin Clark: https://code-cartoons.com/a-cartoon-intro-to-redux-3afb775501a6

TODO: React statistics of usage in projects; compare to other popular libraries and frameworks; why do people use it again?
TODO: Explain create-react-app before coding session