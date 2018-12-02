![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Props, States & Lifecycle Methods

## Learning Goals

* Know how the Component Tree works
* How the DOM is rendered in the browser
* Have a notion of state and props importance
* Know how the Component Lifecycle works

## Introduction - Know how the Component Tree works

**Components are the core building block of React apps.** A typical React app is a set of components which can be represented as a **component tree**. This tree has one root component (usually named "App.js") that contains all child components.

React lets you define components as classes or functions. **Functional components** are functions that return jsx to the. React renders it consequently to the DOM. Functional components`can receive props` but they `don't have state`. This is super important to remember for later, when we'll explain what props and state are.

**Components** defined **as classes** provide more features. The first and the most important feature is **state**. One way to define a React component as a class is by extending with `React.Component`:

```jsx
import React from 'react';

class Welcome extends React.Component {
  render() {
    // return some jsx here
  }
}
```

Here we extend our class (*Welcome*) with `Component` that we imported from `react`. Now we can inherit all its useful properties and methods. We always have to import `React` and `Component` when we want to use a class based Component.

So far this all looks familiar, right? This is the way our `App.js`, the root component, is built.

Lets shake things up a bit. So far we have only one component (*App.js*). We said that React is all about working with a lots of components, so let's create some more. 
Create a folder `components` in the root directory of the project. In that folder, create `User.js`.
`User.js` is going to be an example of the *functional* or so called *stateless* component.

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

Here we can introduce the concept of **props**. Props are the `changes that come from outside of the component` or more visually described, they are `the data passed into the component`.

Import the `<User />` tag in our `App.js` component and make the render method of `App.js` return `<User />`. While your on it, pass some data down to `<User />`.

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
Note that we are inside the functional component and that's why we can refer to props object with just `{props.firstName}`, but if we were inside the class we would have to use `this` keyword so it would be `{this.props.firstName}`.

If rebundle our app now (`npm run webpack`) we can see the changes.

And now if we talk about reusability, we can pass any data to as many `<User />` components as we need and have them rendered onto the DOM.

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

Rebundle your app and check out the changes in the DOM.

So far we've covered `props` and we now know that props allow us to `pass data to components`. Components can not change their own props. Components can however change their own state.

**State is referring to changes within the component.** All these changes will cause the DOM to re-render and possibly to update the view.

The React `Component` class that we always import at the beginning of our code has a special property called `state`. Remember that this also means that we can use state only in components that are classes extended by Component class.

So let's go to our `App.js`, so far the only component built by class that extends `Component` and let's introduce `state` there.

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

If we rebundle our app now, we will not see any difference and we are using the state. If state changes, that will lead to re-rendering and updating the DOM.

To change the state, some kind of event needs to be triggered. Let's create `button` element in our `render()` method inside `App.js` and let's attach the **onClick** listener to it. Also, let's add two new properties inside our state object: `clickCount` and let's set it to `0` and `backColor` set to `yellow`. So at first we want to change the state of our App component and we want to see the changes reflecting on the DOM as we change the state. Let's create `clickHandler()` method that will update the state with every click on the button that has this event attached to it using *onClick* listener. 
So far we passed only *firstName* props to our *User.js* component but we have a lot of information in our *App.js* component so let's update our *User.js*:

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

(Also at this point you can comment out all the old code from the lesson 2 and 3, since that was 'hard coded' for the purpose of explaining how to setup React app and how to use JSX. From now on, we will be dynamically changing our content and state and that will trigger updating the UI/DOM.)

And our updated `App.js` now looks like this:

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

To see the changes, rebundle your app.
Note that we used predefined method **setState()** to update the state. This method is part of the Component's method gallery and it's mandatory to use it when updating the state. **It's wrong to directly change the state, skipping the setState()**.

The last thing we want to cover is reflecting the changes of the state in one component to visually change another component updating its DOM. So let's say we want to change names of our users if the *clickCount* is 5 and also we want to update *backgroundColor* property of the *h2* tag inside `User.js` component on every click, but when *clickCount* is 5 we want to set it back to original color and that is *yellow*. This means we will have to update the state of *App.js* depending of condition and these updates will trigger changes in `User.js` component through the props that are passed. Make sure you add `colorMapper()` method, which will help us to change background color to the random colors.

Let's update *App.js* one more time:

```jsx
// App.js - add changes to clickHandler(), add new colorMapper() method, the rest of the code stays the same

...

colorMapper = () => {
    return "#" + Math.floor(Math.random() * 16777215).toString(16);
}

clickHandler = () => {
    this.setState({
      clickCount: this.state.clickCount+1
    }, ()=>{
        if(this.state.clickCount === 5){
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
    }) 
}
  ...
```
Now if we rebundle our app, we can see the changes. This starts to be interesting right?

The last but definitely not the least important are **lifecycle methods** or so called **lifecycle hooks**. Remember, these methods exist only in `stateful` components, that is classes.

![image](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_e51b95a23ce298a92e62a2d61505b57e.jpg)

As we can see, all the methods can be divided into 4 categories depending on the state of the component when each will be triggered.

### Mounting or First Render

These methods are called in the following order when an instance of a component is being created and inserted into the DOM:

1. **constructor()**
2. **componentWillMount()**
3. **render()**
4. **componentDidMount()**


The `constructor()` get executed first. This is actually the same constructor we saw earlier in classes, so it's the ES6 class feature, it's not something related strictly to React. 
We don't have to use `constructor()` but if we do, we will use it to `pass some props` to it and by default it needs to contain super keyword used as a method that receives props as argument. Also, we may `set the state` in the constructor but don't forget to use `this` keyword with it. The third thing the constructor might be used for is to `bind the methods` to the component.

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

`componentDidMount()` is called in the end just to confirm that this component is successfully mounted. Since the *render()* method is already executed, the **DOM will be already present**. If the DOM is present, it means that you can reference it. If we need to cause some side-effect, like reaching out to fetch some data, this can be good place to do it. We *shouldn't call setState()* here since this will lead to re-rendering of the component.

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