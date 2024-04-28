# React Fundamentals

## Lifecycle Methods in Class Components

[ReactJS Tutorial - 22 - Component Lifecycle Methods at https://youtu.be/qnN_FuFNq2g](https://youtu.be/qnN_FuFNq2g)  
These methods can be classified into **4 phases**:

1. **Mounting methods** are called when an insance of a component is being created and inserted into the DOM.
   - `constructor(props)`
   - `static getDerivedStateFromProps(props, state)`
   - `render()`
   - `componentDidMount()`
2. **Updating methods** are called when a component is being re-rendered as a result of changes to either its props or state.
   - `static getDerivedStateFromProps(props, state)`
   - `shouldComponentUpdate(nextProps, nextState)`
   - `render()`
   - `getSnapShotBeforeUpdate(prevProps, prevState)`
   - `componentDidUpdate(prevProps, prevState, snapshot)`
3. **Unmounting method** is called when a component is being removed from the DOM.
   - `componentWillUnmount`
4. **Error Handling methods** are called when there is an error during rendering, in a life cycle method, or in the constructor of any child component.
   - `static getDerivedStateFromError`
   - `componentDidCatch`

## Mounting Phase Lifecycle Methods

[ReactJS Tutorial - 23 - Component Mounting Lifecycle Methods at https://youtu.be/KDXZibVdiEI](https://youtu.be/KDXZibVdiEI)  
Component `LifecycleA` and `LifecycleB`.

1. `constructor(props)` :star::star::star::star::star:
    - It is a special function that will get called whenever a new component is created.
    - It is the perfect place to **initialize state** and **bind event handlers.**
    - :warning: **You should never cause side effects from within a constructor** like `HTTP requests` or `Ajax`.
    - Bear in mind calling `super(props)` is required to be able to use `this.props` afterward, and `constructor` is the only place where you are expected to change or set the state by directly overwriting the `this.state` property otherwise you have to use `this.setState()` method.

2. `static getDerivedStateFromProps(props, state)` :star:
    - It is a rarely used method to set the state of a component which initial state depends on `props` being passed to the component.
    - Since the method is static you do not have access to `this.setState()` within its body, instead you just have to return an object that represents the new state of the component.
    - Again :warning: do not cause side effects like **fetching data** from an end point.

3. `render()` :star::star::star::star::star:
    - It is the only **required** method in a class component.
    - It only reads `props` and `state` and return `jsx`. It is a **pure function** since it will always return the same `jsx` when passed the same `props` and `state`.
    - :warning: You should never change state, interact with the DOM or make any Ajax calls.
    - Since **children component** may be included within the `render()` method `jsx`, their lifecycle methods are also excecuted (see Component `LifecycleA` and `LifecycleB`).

4. `componentDidMount()` :star::star::star:
    - This method is called only once in the whole life cycle of a component and it is invoqued immediately after all its children components have been rendered to the DOM (**did mount**).
    - :bulb: It is the perfect place to cause side effects. Ex: you can interact with the DOM or perform any ajax call to load data.

  **Important**

  <mark>You should know the order of excecution of all those methods to avoid writing code that might have unexpected behaviour.</mark>

  1. ParentComp: **`constructor`**
  2. ParentComp: **`getDerivedStateFromProps`**
  3. ParentComp: **`render`**
  4. ChildrenComp: **`constructor`**
  5. ChildrenComp: **`getDerivedStateFromProps`**
  6. ChildrenComp: **`render`**
  7. ChildrenComp: **`componentDidMount`**
  8. ParentComp: **`componentDidMount`**

## Updating Phase Lifecycle Methods

[ReactJS Tutorial - 24 - Component Updating Lifecycle Methods at https://youtu.be/DyPkojd1fas](https://youtu.be/DyPkojd1fas)

1. `static getDerivedStateFromProps(props, state)` :star:
    - This static method which receives `props` and `state` as its parameters is called every time the component is re-rendered and returns `null` or an object that represents the updated state on the component.
    - It is used when `state` depends on the `props`.
    - You should never cause any side effect within it.
    - It is one of the rarely used methods in the updating phase.

2. `shouldComponentUpdate(nextProps, nextState)` :star:
   - This method dictates whether a component should re-render or not by returning either `true` or `false`.
   - By default all class component will re-render when changes occur on their `props` or `state`. In this method you can prevent this default behaviour by comparing previous `props` or `state` with next `props` or `state` and let React know whether the component should update or not by returning either `true` or `false`. That is why this method is basically for <mark>**performance optimisation.**
   - You should not cause any side effect or update state within it.
   - It is one of the rarely used methods according to **React** official documentation.

3. `render()` ⭐⭐⭐⭐⭐
    - It is the only **required** method in a class component.
    - It only reads `props` and `state` and return `jsx`.
    - :warning: You should never change state, interact with the DOM or make any Ajax calls.

4. `getSnapShotBeforeUpdate(prevProps, prevState)` :star:
   - This method is called right before the changes from the **virtual DOM** are to be reflected in the **DOM.**
   - You will use it to capture some information from the DOM, Ex: read the user scroll and maintain it after the update.
   - The method will either return null or a value that will be passed as 3rd parameter to the next method.
   - Again it is classified as rarely used method.

5. `componentDidUpdate(prevProps, prevState, snapshot)` ⭐⭐⭐⭐
   - Called after the render is finished in the re-render cycles i.e. the component and all its sub-components have properly rendered after the update.
   - This method is garanteed to be called only once in each re-render cycle, so its suitable for side effects. Ex: making Ajax request after insuring you compared `prevState` with `nextState` to avoid making unnecessary requests.

  :memo:

  <mark>**`render()`** and **`componentDidUpdate(prevProps, prevState, snapshot)`** are the most commonly used methods. The remaining others are reserved to special cases and should be used sparingly. Therefore make sure you know what you are doing if at all you do decide to define them lest you run into unexpected results.</mark>

  1. ParentComp: **`getDerivedStateFromProps`**
  2. ParentComp: **`shouldComponentUpdate`**
  3. ParentComp: **`render`**
  4. ChildrenComp: **`getDerivedStateFromProps`**
  5. ChildrenComp: **`shouldComponentUpdate`**
  6. ChildrenComp: **`render`**
  7. ChildrenComp: **`getSnapShotBeforeUpdate`**
  8. ParentComp: **`getSnapShotBeforeUpdate`**
  9. ChildrenComp: **`componentDidUpdate`**
  10. ParentComp: **`componentDidUpdate`**

## Unmounting Phase Lifecycle Method

Its unique method is **`componentWillUnmount()`**:

- Invoqued immediately before a component is unmounted and destroyed.
- You could use it to cancel network requests, removing event handlers, cancelling any subscriptions and invalidating timers.
- Do not call setState method since component are never re-render after it has been unmounted.

## Error Handling Phase Lifecycle Methods

- **`static getDerivedStateFromError(error)`**
- **`componentDidCatch(error, info)`**

These methods are called either during rendering, in a lifecycle method, or in the constructor of any child component. They are tackled in details in the concept of [**error bounderies in React.**](#error-boundary)

## React Fragments

[ReactJS Tutorial - 25 - Fragments at https://youtu.be/bHdh1T0-US4](https://youtu.be/bHdh1T0-US4)  
(See components: **`FragmentDemo.jsx`**, **`Table.jsx`**, **`Columns.jsx`**).

The React fragment is the **`<React.Fragment>...</React.Fragment>`** component, with its shorthand **`<>...</>`**. In its long form It can accept a **`key={}`** props in the event a list needs to be rendered within the fragment.

## Pure Components

[**ReactJS Tutorial - 26 - Pure Components** at https://youtu.be/YCRuTT31qR0](https://youtu.be/YCRuTT31qR0?t=520)  
See components: **`ParentComp.jsx`**, **`RegComp.jsx`**, **`PureComp.jsx`**.

**Reference equality Master Class by Maximillian Schwarztmüller** <https://youtu.be/9ooYYRLdg_g?t=480>

- Whereas regular React class components are created by extending the **`Component`** class, **pure components** are extensions of the **`PureComponent`** class.

|Regular Component|Pure Component|
|:--------|:----------|
|A Regular component does not implement the `shouldComponentUpdate(nextProps, nextState)` method. It always returns `true` by default|A pure component on the other hand implements `shouldComponentUpdate(nextProps, nextState)` with a **shallow comparison** of `props` and `state`.  ~~No, because the very reason why when the **`memo API`** implements `shouldComponentUpdate` it returns `false` for primitive type props and `true` for complex type props is **strict equality comparison** (`===`)~~|

⚠ **WARNING**

<mark>**When passing props to optimized child component, use spread operator destructuring with caution because it defeats all effect of `React.memo ` API on complex type.**

> 📝 **Shallow Comparison**
>
> - **Primitive Types:**  
> a (SC) b returns `true` if they are of same type and have same value.  
> **Ex:** `string 'vishwas'` (SC) `string 'vishwas'` returns `true`.
> - **Complex Types:**  
> `array A` (SC) `array B` returns `true` if the objects they are references to have both the same value. **Ex:**
>
> ```js
>   const a = { x:1, y:2};
>   const b = { x:1, y:2};
>   const c = a;
>
>   // a === b returns false.
>   // Since every object literal is unique instance of
>   // some memory data.
>   // Whereas
>   // a === c returns true by (SC)
>   // and a (SC) b will return true.
>   
> ```
> **Spread operator**
>
> Passing the `item` prop to the optomized (by `React.memo`) `List` component this way:
>```js
><List key={story.obkectID} item={story} />
>```
> Is not the same as as this way:
>```js
><List key={story.obkectID} item={{...story}} />
>```
> <mark>**They both return object structurally identic, but in the second case an new object is instanciated on the fly at every render and is therefore never the same as the preceding one memoized by `React.memo`. That is the reason why you must avoid passing destructured props to optimized child component.**

- **Pure component** implements `shouldComponentUpdate(nextProps, nextState)` with a **shallow comparison** of `prevProps` with `newProps` and `prevState` with `newState` and **only when there is a difference will the component re-render.**
- In the tutorial examples if you turn the `ParentComp` component into `PureComponent` there should no longer be any update since as long as **shallow comparison** is concerned there is **no props change** in this example, because always bear in mind that what ever happens in sub-components they never re-render unless the parent component renders first.

- **Use case**  
  **Pure component** are mostly usefull for **performance boost.** Ex: when implementing a list of 50 items it will prevent React from making unnecessary updates.

- **Key point**  
  **Never mutate array or object in state or props.** ~~Since `shouldComponentUpdate` method only checks for the **reference**(which never change in case of mutation) not for the very memory data~~ (:star:🤔🤔🤔**It might be React checks for reference equality on the whole component but apllies strict equality comparaison on its `states`, `props`, and `context`** 🤔🤔🤔), no update will occur after performing your mutation (like `list.push(newItem)`). You should always return a new object.

  ```js
    this.setState(() => {
        return list.push(newItem);
    })  // Nothing will happen
    // The proper way
    this.setState(() => {
        return { newItems };
    })
  ```

- :bulb: **TAKE AWAY**  
  When implementing **pure component** make sure all children components are also **pure components** to avoid unexpected behabiour.  
  Never mutate the state. Always return an object that reflects the new state.  
  <mark>It is always safe to opt for **regular components** unless you notice a performance head in some components.</mark>

## React.memo

[**ReactJS Tutorial - 27 - memo at https://youtu.be/7TaBhrnPH78**](https://youtu.be/7TaBhrnPH78)  
See components: **`ParentComp.jsx`**, **`MemoComp.jsx`**.

- **Memo** is to **functional component** what **pure component** is to **class-based component.** It enables functinonal components to check for changes to props and state and to prevent re-render if unchanged.
- It works by importing the `React.memo()` **higher order component** in the `export default` statement and passing it the functional component we want to behave like a class-based pure component.

  ```js
    export default React.memo(MemoComp);
  ```

- A **higher order component** is a component that takes as argument another component and return an enhanced version of this component. In this case a **pure-component-like functional component** capable of avioding re-render when there are no changes to its props or state.

## React Refs

**ReactJS Tutorial - 28 - Refs <https://youtu.be/FXa9mMTKOu8>**  
See component: **`RefsDemo.jsx`**

While React mostly uses **declarative programming**, sometimes **imperative programming** is required when you need to directly access some node to **explicitely** programm its behaviour. That is where **React ref** comes into action.

`useRefs` hook has two main purposes:

1. **Holding a reference to a `DOM node`**  
   <span style="background-color: yellow;">Here, the most important step is to set the **`ref` reserved attribute** on the element you need to code imperatively. Afterwards there are two technics using either the `reference object` or `callback refs`.</span>

2. **Storing any `mutable value`**  
   When you need a value to persist through re-render, and mutate (be updated) without causing any re-render of the component. <https://youtu.be/LWg0OyZQffc?t=400>

### Hold a reference to a `DOM node`

#### Using the reference object

1. Set the **ref attribute value** to the **reference object** return by **`React.createRef()`** and previously assigned to an instance field. This **reference object** has a **`current` property** which points back to the element node on which the **ref attribute** is set.

   ```js
   this.nodeRef = React.createRef();
   ```

   ```js
   <jsxElement ref={this.nodeRef}>...</jsxElement>
   ```

2. Then you can use the **current property** of the **reference object** to **imperatively program** the target element

   ```js
   componentDidMount() {
    this.nodeRef.current.focus();
   }
   ```

#### Using a callback Refs

1. Set the **ref attribute value** to a **callback Refs** previously created as **an instance method** which sole purpose is to update an **instance field** previously initialized as `null`.

   ```jsx
   this.callbackRef = null;   //(1)
   ```

   ```jsx
   this.setCallbackRef = (element) => {  //(2)
    this.callbackRef = element
   }
   ```

   ```jsx
   <jsxElement ref={this.setCallbackRef}>...</jsxElement>    //(3) 
   ```

2. **How it works**  
   Since React will call the **callback ref method** with the **Dom Element implicit argument** as soon as it mounts, and call it again with null when it unmounts, you always have to check for the existence of a non-null value on the callback property.

   ```js
   componentDidMount() {
    if (this.callbackRef) {
      this.callbackRef.focus();
    }
   }
   ```

#### Refs with Class Components

[**ReactJS Tutorial - 29 - Refs with Class Components https://youtu.be/8aCXVC9Qmto**](https://youtu.be/8aCXVC9Qmto)  
See components:  **`Input.jsx`**, **`FocusInput.jsx`**

While you can use React Ref to imperatively program an HTML Element within a class component, <span style="background-color: yellow;">it is also possible to do the same on another child class component from the parent class component using a first ref to fetch the target element within the child component and a second ref to fetch the child component within the parent component.</span> It can be achieved in four steps:

1. Within the **child class component** create a `ref` and attach it to the **target HTML element**.
2. Within the **child class component** create a method that will use the `ref` to imperatively program the target HTML element.
3. Within the **parent class component** create a  `ref` and attach it to **child class element**.
4. Within the **parent class component** create a method that will use the `ref` to fetch the **child class element** and trigger **imperatively** its method created on step 2.

#### Forwarding Refs

[**ReactJS Tutorial - 30 - Forwarding Refs https://youtu.be/RLWniwmfdq4**](https://youtu.be/RLWniwmfdq4)  
See components:  **`ForwardInput.jsx`**, **`ForwardParentInput.jsx`**

**React ref forwarding** is a way to directly create a `ref` to **child component inner HTML Element** from within the **parent class component** by forwarding the parent `ref` to the child component which in such case must be declared with `const` as the **arrow function component** returned by `React.forwardRef()`

```js
   import React from 'react'

   const ChildComponent = React.forwardRef((props, ref) => {
    return (
      <div>
          <input type="text" ref={ref} />
      </div>
    )
   });

   export default ChildComponent
```

`React.forwardRef()`  forwards the parent `ref` as a second argument to the child functional component and returns the component with the attached `ref`.

<mark>**Ref forwarding is rarely used.**

## Portals

[**ReactJS Tutorial - 31 - Portals** https://youtu.be/HpHLa-5Wdys](https://youtu.be/HpHLa-5Wdys)  
See components:  **`PortalModal.jsx`**

When you need a child component to be rendered out of the React component tree and directly in the DOM you can achieve it in the following steps:

1. Import **`ReactDOM`**

   ```js
      import ReactDOM from "react-dom";
   ```

2. Fetch the DOM node you need to insert the child component into

   ```js
   // Example:
   const portal = document.getElementById("portal-root");
   ```

3. Within the render method of the child component call the **`createPortal()`** passing it **the whole child component JSX** as first argument and the portal node as second argument and return the result.

   ```js
   render() {
      return ReactDOM.createPortal( <childComponent />, portal );
   }
   ```

:warning: Even though the child component is inserted in a different DOM sub-tree than its React parent component, **child component events keep bubbling through its React parent component.** ([See example https://codepen.io/gaearon/pen/jGBWpE](https://codepen.io/gaearon/pen/jGBWpE) )

## Error Boundary

[ReactJS Tutorial - 32 - Error Boundary https://youtu.be/DNYXgtZBRPE](https://youtu.be/DNYXgtZBRPE)  
See components:  **`Hero.jsx`,** **`ErrorBoundary.jsx`**

A class component that implements either one or both of the life cycle methods `getDerivedStateFromError` or `componentDidCatch` becomes an **error boundary.**

The `static getDerivedStateFromError` method is used to render a fallback UI after an error is thrown and `componentDidCatch` method is used to log the error information.

When an error occurs wherever in a React tree React just unmount the whole tree(component). If for example an error is thrown in only one item of a one thousand-item commercial page and you don't want the whole site to crash **error boundary** allows you to catch the error and display a fallback UI in its place at the **production level** only according to its intended behaviour. At the **development level,** **error boundary** keeps unmounting the whole component.

1. First you create a class component as an error boundary

   ```js
      import React from 'react';

      class ErrorBoundary extends React.Component {
         constructor(props) {
            super(props)
            //(2) Set state to check for error existence
            this.state = {
               hasError: false,
            }
         }
         //(1) Call error handling lifcycle method that update state in case of error
         static getDerivedStateFromError(error) {
            return { hasError: true }
         }
         //(5) Optionally call the second method
         componentDidCatch(error, info) {
            // Example
            console.log(error);
            console.log(info);
         }
         render() {
            //(3) Display fallback UI
            if(this.state.hasError) {
               return <h3>Something went wrong</h3>;
            }
            //(4) Otherwise display children component(s) passed through props
            return this.props.children;
         }
      }
   ```

2. Insert the error boundary component in the component on which sub-components you need to catch errors and pass those sub-components as children of the error boundary component.

   ```js
      const ParentComponent = () => {
        return (
          <div>
            <ErrorBoundary>
              <SubComponent name={'A'} />
              <SubComponent name={'B'} />
              <SubComponent name={'C'} />
            </ErrorBoundary>
          </div>
        )
      }
   ```

   The placement of the error boundary matters as it decides whether the whole application should have the fallback UI or just the broken sub-components.

   ```js
      const ParentComponent = () => {
        return (
          <div>
            <ErrorBoundary>
              <SubComponent name={'A'} />
            </ErrorBoundary>
            <ErrorBoundary>
              <SubComponent name={'B'} />
            </ErrorBoundary>
            <ErrorBoundary>
              <SubComponent name={'C'} />
            </ErrorBoundary>
          </div>
        )
      }

- In **development mode** `componentDidCatch` is redundant since React logs by default all those error and info in the console

Error boundaries catch errors **during rendering**, **in life cycle methods** and **in the constructor of the whole tree bellow them** however **they do not catch errors inside event handlers** so if you need to handle errors in a `onClick={}` event handlers for example you just have to use a regular `try catch` statement.

## Higher Order Components

[ReactJS Tutorial - 34 - Higher Order Components https://youtu.be/rsBQj6X7UK8](https://youtu.be/rsBQj6X7UK8)  
See components:  **`ClickCounter.jsx`,** **`HoverCounter.jsx`** and  
Higher-order component: **`withCounter.jsx`**

- **Why ?**  
  It happens in React multiple components partially implement the same functionality, and as you insert several of these components in React tree you need to avoid the redundancy of typing the same functionalities over and over again.

   One solution is to apply **state lifting,** but it works as long as the components with the same functionalities are the children of the same parent component. So how do you distribute the same functionalities among multiple components scattered throughout the React tree? By using a **higher-order component.**

- **Definition**  
  **Higher-order component** is a pattern where a function takes a component as an argument and returns a new component.

  ```js
  // It works on this principle
  const NewComponent = higherOrderComponent(OriginalComponent);
  // Or
  const EnhancedComponent = higherOrderComponent(OriginalComponent);
  ```

- **How?**  
  First create the **higher-order component**

  ```js
  import React, {Component} from 'react';

  const withCommonFunctionality = (WrappedComponent, option) => {
    class WithCommonFunctionality extends Component {
      constructor(props) {
        super(props)
        this.state = {
          // some property
        }
      }

      commonFunctionality = () => {
        this.setState((prevState) => {
            return (
            // Update state
          );
        })
      }

      render() {
        return (
          <WrappedComponent
              someProp={this.state}
              commonFunctionality={commonFunctionality}
              { ... this.props }
          />
        );
      }
    }

    return WithCommonFunctionality;
  }
  // Export the higher-order component
  export default withCommonFunctionality;
  ```

  - 📝
  You could name them as you please but by convention while components use **PascalCase**, higher order components use **camelCase**.  
  Higher order component names are made up of **`with`** + **`SharedFunctionality`**.  
  In the higher order component the **`OriginalComponent`** is named **`WrappedComponent`** and the **`NewComponent`** just use the higher order component name in **PascalCase** as **`WithSharedFunctionality`**.
  - Notice the `{ ... this.props }` props. Since the parent component no longer access directly the child component but through the mediation of the higher order component, in the event you need to pass props from parent to children make sure to keep to communication channel open by spreading in the returned WrappedComponent any potentially received props of the enhancer component.

  Secondly define the component which needs the shared functionality, make use of the fuctionalities recieved through props and finally instead of exporting it as usual export its enhanced version.

  ```js
  import React, { Component } from 'react'
  import withSharedFunctionality from './withSharedFunctionality';

  // (FunctionalityUser is the WrappedComponent)
  class FunctionalityUser extends Component {
    render() {
      const {functionality} = this.props;
      return (
        <div>
          { /* Make use of the props functionalities */ }
        </div>
      )
    }
  }
  export default withSharedFunctionality(FunctionalityUser, option)
  /* 
  VERY IMPORTANT

  While in the importing module (parent commponent)
  enhanced component are imported with statement:

  import FunctionalityUser from './component/FunctionalityUser'

  Keep in mind that 'FunctionalityUser' is no longer
  the FunctionalityUser direct class since its instance
  is enhanced through the higher order component and the default
  export is that enhanced sub-class not a FunctionalityUser class.
  So why does it work?

  Because contrary to named exports, default export can be retrieved
  with any identifier name.
  */
  ```

- **Summary**  
  A higher order component is a functional component which takes a component to enhance as first argument and returns a class component which class and instance methods and  properties are the functionalities to share and are distributed by passing them as props to the component to enhance in the return statement of its render method. Optionally, this HOC takes as second parameter an argument received from the wrapped component and used to fine tune the functionalities to distribute.

## Render Props

**ReactJS Tutorial - 37 - Render Props, <https://youtu.be/NdapMDgNhtE>, <https://youtu.be/EZil2OTyB4w>**  
See components: **`ClickCounterTwo.jsx`, `HoverCounterTwo.jsx`, `User.jsx`, `RenderProps.jsx` and `App.jsx`.**

Apart from **HOC** there is a second technic to share functionality called **render props** by **using a props whose value is a function.**

- Set up first the **SECOND** phase  
  Create a class component which perform the render props technic. In the body of the class define the shared functionalities and in the render method return statement make sure to call the render prop function received from its parent, and passing it as arguments the shared functionalities.

  ```js
  import React, { Component } from 'react'

  class ShareFuncComponent extends Component {

    // Define the shared functionalities

    render() {
      return (
        <React.Fragment>
          {this.props.render(this.state.sharedState, this.sharedMethod)}
        </React.Fragment>
      )
    }
  }

  export default ShareFuncComponent
  ```

- Set up the **FIRST** phase  
  In the parent component of the component which needs the shared functionalities import and return the render props components and pass it a prop optionally called `render` which value must be a function. The `render` function has as many parameters as are functionalities to be shared and it must return the component which needs the shared functionalities and in turn pass it as props the parameters received from the parent component and turned into arguments by the render props component.

  ```js
  class ParentComponent extends Component {

    // Define the shared functionalities

    render() {
      return (
        <div>
          <ShareFuncComponent render={(sharedFunctionality) => (
            <ChildComponent sharedFunctionality={sharedFunctionality} />
          )} />
        </div>
      )
    }
  }

  export default ParentComponent
  ```

- The shared functionality carrier function is called `render` by convention, however, it can be named any other way. There is even another technic which discards the named props called `render` by directly using a `reserved props` called `children`.

  ```jsx
    render() {
      return (
        <div>
          <ShareFuncComponent>
            {(sharedFunctionality) => (
              <ChildComponent sharedFunctionality={sharedFunctionality} />
            )}
          </ShareFuncComponent>
        </div>
      )
    }
  ```

  In the `ShareFuncComponent`.

  ```js
    render() {
      return (
        <React.Fragment>
          {this.props.children(this.state.sharedState, this.sharedMethod)}
        </React.Fragment>
      )
    }
  ```

## Context API

**ReactJS Tutorial - 38, 39, 40 - Context, <https://youtu.be/j3j8St50fNY>, <https://youtu.be/lTjQjWemKgE>, <https://youtu.be/A9WlkhdLnn0>.**  
See components sub-tree: **`App.jsx` ⇒ `userContext.jsx` ⇒ `ComponentC.jsx` ⇒ `ComponentE.jsx` ⇒ `ComponentF.jsx`**

**The context API is not optimized for high frequency changes (see Redux). It is better suited for static value which updates are propagated through subscriptions.**

When a piece of information is needed inside a child component you can make use of `props` to pass it from the parent component. In the case grand-child component needs it there is no hindrance to send it from parent component through child component before receiving it by props in the grand-child component.

It happens that piece of information is needed several levels down the React component tree and you realize how tediously cumbersome it would be to manually drill it down every single level of the component tree. That is where `React.createContext()` comes to the rescue.

- Create a context object with an optional default value and either export it or destructure it to export its built-in `Provider`and `Consumer` components.
  
  ```js
  /** 
   * CREATE A CONTEXT
   * Either in the older ancestor provider module
   * which use to be the App component or
   * in a separate module
  */

  import React from "react";

  const UserContext = React.createContext(optionalValue);

  const Provider = UserContext.Provider;
  const Consumer = UserContext.Consumer;

  export { Provider, Consumer };
  ```

- (1) If you want to specify a value, import the `Provider` component in the parent component and wrap it around the most likely elder component to be ancestor of potential consumer descendant components. Pass the data to consume to the `Provider` component as value of the `value` `props`.

  ```js
  // It would be perfectly fine to create the context here
  // besides providing and finally exporting it.

  import { Provider } from './component/userContext'

  const App = () => {
    return (
      <div>
        <Provider value='Cake' >
          <ComponentC />
        </Provider>
      </div>
    )
  }
  ```

  (2) If the value to provide is the default value defined in the first step skip to the third step without importing the `Provider` component.

  ```js
  const App = () => {
    return (
      <div>
        <ComponentC />
      </div>
    )
  }
  ```

- In the context-value-consumer component render the `Consumer` component wrapping an arrow function whose argument is the `value` to consume and returns the context-value-consumer component JSX consuming that value.

  ```js
  import { Consumer } from './userContext'

  class ComponentF extends Component {
    render() {
      return (
        <Consumer>
          {(value) => ( <div>ComponentF {value}</div> )}
        </Consumer>
      )
    }
  }
  ```

- The optional value is used only if there is no Provider component with specific value in the component tree above the consumer component.

Another technic use the `contextType` property. First you must export the whole context object

  ```js
  import React from "react";

  const UserContext = React.createContext();

  const Provider = UserContext.Provider;
  const Consumer = UserContext.Consumer;

  export { Provider, Consumer };
  export default UserContext;
  ```

Secondly inside the value-receiving component create the `contextType` static field, assign it the context object and within the render method JSX call the context value using the reserved `context` property.

```js
import UserContext from './userContext'

class ComponentE extends Component {

  static contextType = UserContext
  
  render() {
    return (
      <div>
        ComponentE { this.context }
      </div>
    )
  }
}
```

:memo: Although the `contextType` seems more handy it has two limitations, (1) it only works with **class components** (2) it enables you to subscribe to only one context.

To consume multiple contexts the built-in components of the context object are required in the following way:

```js
const Content = () => {
  return (
    <ThemeContext.Consumer>
      {theme => (
        <UserContext.Consumer>
          {user => (
            <ProfilePage theme={theme} user={user} />
          )}
        </UserContext.Consumer>
      )}
    </ThemeContext.Consumer>
  );
}
```

## HTTP and React

**ReactJS Tutorial - 41 - HTTP and React <https://youtu.be/GTmjthNvrxY>**  
See components: **`PostsList.jsx`, `PostForm.jsx`**

Since **React** is a library exclusively concern with User Interface to make HTTP request you could either use the **Fetch API** or install an **HTTP Library package** like **Axios**.

```terminal
npm install axios
```

or

```terminal
yarn add axios
```

:warning: Afterward never forget to reload the server :warning:

Make your request from the apropriate life cycle `componentDidMount` to make them once or `componentDidUpdate` to make them after every update. You can even make them from element event handler like the **submit handler of a form element** :warning: without ever forgeting **to cancel default form behaviour** since **React** must be let in charge of page refresh.:warning:
