![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Props, States & Lifecycle Methods

## Learning Goals

* Know how the Component Tree works
* Understand state and props
* Know how the Component Lifecycle works

## Know how the Component Tree works

**Components are the core building block of React apps.** A typical React app is a set of components which can be represented as a **component tree**. This tree has one root component that contains all child components. The root component is called `App.js` by convention.

React lets you define components as classes or functions. **Functional components** are functions that return jsx. React renders it consequently to the DOM. Functional components`can receive props` but they `don't have state`. This is important to remember for later, when we'll explain what props and state are.

**Class based components** provide more features. The first and the most important feature is **state**. One way to define a React component as a class is by extending it with `React.Component`:

```jsx
import React from 'react';

class Welcome extends React.Component {
  render() {
    // return some jsx here
  }
}
```

Here we extend our class (*Welcome*) with `Component`, which we imported from `react`. Now we can inherit all its useful properties and methods. We always have to import `React` and `Component` when we want to use class based components.

So far this all looks familiar, right? This is the way we've built our root component before.

Lets shake things up a bit. So far we've only one component (*App.js*). We said that React is all about working with components, so let's create some more. 
Create a folder `components` in the root directory of the project. In that folder, create `User.js`.
`User.js` is going to be an example of the *functional* or *stateless* component.

```jsx
// User.js
import React from "react";

const user = () => {
    return (
        <h2>
            Hello, user!
        </h2> 
    )
}

export default user;
```

For now you can leave all the code in the App.js. We will restructure it step by step and make it more useful.

Here we can introduce the concept of **props**. Props are the **changes that come from outside of the component** or put differently, they are **the data passed into the component**.

Import the `<User />` tag in our `App.js` component and make the render method of `App.js` return `<User />`. While you're on it, pass some data down to `<User />`. Do you see how `<User />` has become a child component of `<App />`?

```js
// App.js
...
return (
    <div className="App">
        <User firstName="Harper"/>     
    </div>
);
```

And after that let's update our `User.js` component:

```jsx
...
const user = (props) => {
    return (
        <h2>
            Hello, {props.firstName}!
        </h2> 
    )
}
...
```
We can refer to the props with `{props.firstName}`. If we would use a class based component we would have to use the `this` keyword: `{this.props.firstName}`.

We can pass different data to as many `<User />` components as we need. This enables us to reuse the same component multiple times.

```js
// App.js
...
return (
    <div className="App">
        <User firstName="Harper"/>  
        <User firstName="Ana"/>     
    </div>
);
```
Rebundle your our app now (`npm run webpack`) to see the changes.

So far we've covered `props` and we now know that props allow us to `pass data to components`. Components can not change their own props. however components can change their own state.

**State refers to changes within the component.** All these changes will cause the DOM to re-render and to update the view if necessary.

The React `Component` class that we always import at the beginning of our code has a special property called `state`. Remember that this also means, that we can use state only in components that are classes extended by the Component class.

`App.js` is so far the only class based component that extends `Component`. It's time to give it some state.

```jsx
// App.js

class App extends Component {
  
  state = {
    userA: {
            firstName: 'Harper',
            lastName: 'Perez',
            avatarUrl:'http://www.kodlamaker.com/wp-content/uploads/2015/10/codingforkids.png'
    },
    userB: {
            firstName: 'Ana',
            lastName: 'Hello',
            avatarUrl:'https://s3.amazonaws.com/owler-image/logo/ironhack_owler_20180828_221413_original.png'
    }
  }
  render(){
      ...
      return (
        <div className="App">
            <User firstName={this.state.userA.firstName} />
            <User firstName={this.state.userB.firstName} />
        </div>
      )
  }
}

export default App;
```

If we rebundle our app now, we will not see any difference. Only if the state changes the DOM wil re-render.

To change the state, some kind of event needs to be triggered. Let's create a `button` element in `App.js` and attach the **onClick** listener to it. Also add two new properties inside our state object: `clickCount` and `backColor`. Set the first to `0` and the latter to `yellow`. 

Let's create the `clickHandler()` method that will update the state with every click.

Our updated `App.js` now looks like this:

```jsx
class App extends Component {
  
    state = {
        userA: {
                firstName: 'Harper',
                lastName: 'Perez',
                avatarUrl:'http://www.kodlamaker.com/wp-content/uploads/2015/10/codingforkids.png'
        },
        userB: {
                firstName: 'Ana',
                lastName: 'Hello',
                avatarUrl:'https://s3.amazonaws.com/owler-image/logo/ironhack_owler_20180828_221413_original.png'
        },
        clickCount: 0,
        backColor: 'yellow'
    }

    clickHandler = () => {
        this.setState({
        clickCount: this.state.clickCount+1
        })

    }

    render() {
        return (
        <div className="App">
            <h1> Hello Ironhackers! </h1>
            <p>Count is: {this.state.clickCount}</p>
            <button onClick={this.clickHandler}>Click me</button>
            
            <User theColor={this.state.backColor} firstName={this.state.userA.firstName} lastName={this.state.userA.lastName} image={this.state.userA.avatarUrl} />
            <User firstName={this.state.userB.firstName} lastName={this.state.userB.lastName} image={this.state.userB.avatarUrl} />     
        </div>
        );
    }
}

```

Note that we're passing `backColor` to `<User />` as a prop. We're also passing other data out of `<App />`'s state to `<User />`, like `image` and `avatarUrl`.

```jsx
// User.js
import React from "react";

const user = (props) => {
    return (
        <div>
            <h2 style={{backgroundColor:props.theColor}}>
                Hello, {props.firstName} {props.lastName}!
            </h2> 
            <img src={props.image} width='370' height='300' />
        </div>
    )
}

export default user;
```

To see the changes, rebundle your app.
Note that we've used the predefined method **setState()** to update the state. It's mandatory to use when updating the state. **Do not change the state directly. Always use setState()**.

Okay okay, so every time we click on a button a counter gets incremented. Veeeeerry impressive. Let's do something more visual. If the counter reaches 5 the names of the users change. At the same time, the background color of `<h2>` changes on every click to some random color. The helper function `colorMapper` will generate random colors for us.

Update *App.js* one more time:

```jsx
// App.js - add changes to clickHandler(), add new colorMapper() method, the rest of the code stays the same

...

colorMapper = () => {
    return "#" + Math.floor(Math.random() * 16777215).toString(16);
}

clickHandler = () => {
    this.setState({
      clickCount: this.state.clickCount+1
    })
    if(this.state.clickCount % 5 === 0){
        this.setState({
            userA: {
                    firstName: 'Jon',
                    lastName: 'Doe',
                    avatarUrl:'http://www.kodlamaker.com/wp-content/uploads/2015/10/codingforkids.png'
            },
            userB: {
                    firstName: 'Susanne',
                    lastName: 'Smith',
                    avatarUrl:'https://s3.amazonaws.com/owler-image/logo/ironhack_owler_20180828_221413_original.png'
            },
            backColor: 'yellow'
        })
    } else {
        this.setState({
            backColor: this.colorMapper()
        })
    }
}
  ...
```
Now if we rebundle our app, we can see the changes. Are you slightly more impressed?

Last but not least: the **lifecycle methods** or so called **lifecycle hooks**. Remember, these methods exist only in `stateful` components.

![image](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_e51b95a23ce298a92e62a2d61505b57e.jpg)

As we can see, the methods can be divided into 4 categories.

### Mounting or First Render

These methods are called in the folowing order when an instance of a component is created:

1. **constructor()**
2. **componentWillMount()**
3. **render()**
4. **componentDidMount()**


The `constructor()` get executed first. This is the same constructor we saw earlier in classes, so it's the ES6 class feature, it's not something related strictly to React. 
We don't have to use `constructor()` but if we do, we will use it to `pass some props` to it. It needs to contain the super keyword. Also, we may `set the state` in the constructor but don't forget to prefix it with the `this` keyword. The third thing the constructor might be used for is to `bind the methods` to the component.

```js
constructor(props){
    // this 'props' is actually the object inherited from the Component imported from 'react' library
    super(props);
    this.state = {
        // some property-values pairs go here
    }
}
```
The Constructor is the only place where *you should assign `this.state` directly*. In all other methods, you need to use `this.setState()` instead.

After constructor(), the `componentWillMount()` method gets executed. This component is part of React Component's method library, but it's rarely used nowadays. It still exists mainly because of historical reasons. If you decide to use it, you can do it to `update the state`.

The `render()` method is next in line when we mount the component. So far we saw that render() actually renders the DOM but at this point that's not the case just yet. `render()` structures and prepares JSX and how it's supposed to look like when DOM gets rendered later.

`componentDidMount()` is called in the end to confirm that this component is mounted successfully. Since the *render()* method is already executed, the **DOM will be already present**. If the DOM is present, it means that you can reference it. If we need to cause some side-effect, like reaching out to fetch some data, this can be good place to do it. We *shouldn't call setState()* here since this will lead to re-rendering of the component.

In between *render()* and *componentDidMount()* all the other component will render and all their mounting methods will happen.

### Updating

An update can be caused by changes to props or state. The most commonly used methods when the component is being re-rendered are: 

1. **componentWillReceiveProps()**
2. **shouldComponentUpdate()**
3. **componentWillUpdate()**
4. **render()**
5. **componentDidUpdate()**

`componentWillReceiveProps(nextProps)` is the method that will receive the updated props after they've changed. Let's say we want to delete something like an instance of an array. The newly updated array will be passed to componentWillReceiveProps(nextProps) as `nextProps`. This method will execute only if the props are changed, so it will be triggered by changes from outside of the component. It doesn't execute if the change is triggered by inner state changes.

`shouldComponentUpdate(nextProps, nextState)` is the only lifecycle method that returns *true* or *false*. It can be used to increase performance. If it returns true, it will execute the render() method and update the DOM. However, if it returns false, the render() method is cancelled. Cancelling the render() method will save the browser work, because it does not have to re-render the component. For example, in this method we can compare the current (nextProps.something) with an already existing props (this.props.something) and decide if we should proceed and trigger a DOM change. An example would be a situation wherein a stateful component receives a lot of props but we want to keep track of only one. Depending if the prop of interest changed, we decide if the DOM should update.

`componentWillUpdate()` is the next one and it will be triggered if *shouldComponentUpdate()* returns true.

`render()` will do the same as usual, sets up JSX side of the component.

The last one in the *update* part is `componentDidUpdate()` and it does pretty much the same as *componentDidMount()*. Here we shouldn't update the state but we can fetch some external data if we need them.

### Unmounting

`componentWillUnmount()` is called when a component is being removed from the DOM.

 
## React.Component lifecycle cheatsheet

![](https://user-images.githubusercontent.com/568638/40828476-0667470c-6581-11e8-8f0b-5b4627676505.png)


## Summary

In this learning unit we covered the core of the React magic world - we learned about influencing components from outside using *props*, referring to changes within the component using *state* and we touched upon the basics lifecycle methods. 

## Extra Resources

- [`React.Component` on reactjs.org](https://reactjs.org/docs/react-component.html)
- [How to use React Lifecycle Methods](https://codeburst.io/how-to-use-react-lifecycle-methods-ddc79699b34e)
- [(Video) Component Lifecycle](https://www.youtube.com/watch?v=Oioo0IdoEls)
- [`SyntheticEvent` on reactjs.org](https://reactjs.org/docs/events.html)