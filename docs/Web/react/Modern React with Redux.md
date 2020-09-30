# Modern React with Redux

## JSX vs HTML

- Adding custom styling to an element uses different syntax.
- Use className instead of class due to class is JS keyword
- JSX can reference JS variables(**you can't show JS object as text**)

**html**

```html
<div class="div" style="background-color:red;"></div>
```

**JSX**

```jsx
<div className="div" style={{ backgroundColor:'red' }}></div>
```

## Rules of State

- Only usable with class components(technically can be used with functional components using the 'hook' system)
- You will confuse props with state :(
- 'State' is a JS object that contains data relevant to a component
- Updating 'state' on a component causes the component to (almost) instantly rerender
- State must be initialized when a component is created
- State can **only** be updated using the function 'setState'

## Component Lifecycle

1. `constructor`
2. `render`
3. Content visible on screen
4. `componentDidMount`
5. Sit and wait for updates...
6. `componentDidUpdate`
7. Sit and wait until this component is not longer shown
8. `componentWillUnmount`

- `componentDidUpdate` invoked right after `render`
- do data loading at `componentDidMount` stage although you can 100% do this in `constructor` stage
- other lifecycle methods(rarely used)
  - shouldComponentUpdate
  - getDerivedStateFromProps
  - getSnapshotBeforeUpdate

## Alternate State Initialization

```js
class App extends React.Component {
    constructor(prop) {
        super(props);
        this.state = {
            value: null,
        };
    }
}
```

```js
class App extends React.Component {
    state = {
                value: null,
            };
    }
```

## Adding Some Styling

you could create specific CSS file for a react component and import it, the webpack will do the rest of things.

keep component name, css file name and css class name the same is a good convention. ex: SeasonDisplay.js, SeasonDisplay.css, season-display(classname)

## Specifying Default Props

```js
XXX.defaultProps = {
    a: 'xxx'
};
```

## Avoiding Conditionals in Render

Use helper render function to keep DRY

```js
renderContent() {
    // retrun some JSX
}

render() {
    return {
        <div>
        {renderContent()}
        </div>
    }
}
```

## Solving Context('this') Issues

1. function `bind` in constructor
2. inline arrow function
3. outline arrow function

## Keys in Lists

- list is just a concept not really have to be `<li>`
- only have to assign the `key` to the root element

## Grid CSS

when you apply some styling based upon the content of each component, like makeing a grid styling with different grid-span for images, you usually have to wirte some JS to do this.

## Accessing the DOM with Refs

- Gives access to a single DOM element
- We create refs in the constructor, assign them to instance variables, then pass a particular JSX element as props

```js
class ImageCard extends React.Component {
    constructor(props) {
        super(props)

        this.imageRef = React.createRef();
    }
    componentDidMount() {
        // do something on this.imageRef.current
    }
    return (
        <div>
            <img ref={this.imageRef}>
        </div>
    )
}
```

## Let's Test Your React Mastery!

### Component Design

try to separate components and draw the component hierarchy

### Scaffolding the App

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App';

ReactDOM.render(
    <App />,
    document.querySelector('#root');
)
```

### Reminder on Event Handlers

set state at input element's value because we want to store the data inside the component and not inside the DOM

### Handling Form Submittal

prevent default behavior on form element

### Accessing the Youtube API

remember the API key will send to the user because it will be used inside of the browser. So take care of security issues.

### Styling a List

You can switch out the actual element type that we're using and just apply the same class name and it will still do the appropriate styling to it in sematic UI.

tips: use the separate css file to custmize styling and keep the root element of component using the same className as the css file name.

## What is Redux?

State management library



## Resources

[Diffchecker](https://www.diffchecker.com/)

[CODEPEN](https://codepen.io/)

https://semantic-ui.com/

faker.js - generate massive amounts of realistic fake data in Node.js and the **browser**

unsplash.com

AXIOS