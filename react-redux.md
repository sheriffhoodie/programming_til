## React/Redux Questions and Topics

### Advantages and Features of React

- It uses the virtual DOM instead of the real DOM.
- It uses server-side rendering.
- It follows uni-directional data flow or data binding.
- It increases the application’s performance
- It can be conveniently used on the client as well as server side
- Because of JSX, code’s readability increases
- React is easy to integrate with other frameworks like Meteor, Angular, etc
- Using React, writing UI test cases become extremely easy

### Limitations of React
- React is just a library, not a full-blown framework. It only handles views, unlike Angular which is a complete MVC framework.
- Its library is very large and takes time to understand
- It can be little difficult for the novice programmers to understand
- Coding gets complex as it uses inline templating and JSX

### What is JSX?
JSX is a shorthand for JavaScript XML. This is a type of file used by React which utilizes the expressiveness of JavaScript along with HTML like template syntax. This makes the HTML file really easy to understand. This file makes applications robust and boosts its performance. Browsers can only read JavaScript objects but JSX in not a regular JavaScript object. Thus to enable a browser to read JSX, first, we need to transform JSX file into a JavaScript object using JSX transformers like Babel and then pass it to the browser.

### How do you implement highly performant React components?
- First, measure where the wasted time is - use tools like react-addons-perf to get an overview of your app’s overall performance or react-perf-tool which creates graphs of measured performance for your components.
- To prevent React from running the render-diffing process on ALL components every time, can use the life-cycle method shouldComponentUpdate(nextProps, nextState) in each component. When this function returns true for any component, it allows the render-diff process to be triggered while simply returning false will prevent the component from being re-rendered at all.
- Use React.PureComponent, which makes a shallow comparison of nextProps and props and nextState and state.
- Use immutable data structures, which will copy an object and compare the two for changes, instead of changing the original object.
- Use production build to avoid unnecessary checks
- Better to bind functions early in the constructor because binding them inside the render function will cause render() to create a new function on every render.
- Enable Gzip on your web server to reduce the size of transferred HTTP response by up to 90%

### Why is immutability important in React/Redux applications?

Immutability increases predictability, performance (indirectly) and allows for mutation tracking. Mutation hides change, which creates unexpected side effects and nasty bugs. With immutability enforced you can keep your application architecture and mental model simple. This makes it very easy to see if anything has changed. In a React component, you can check to see if the state is identical by comparing state objects and prevent unnecessary rendering. Redux has a single immutable state tree (the store) where all state changes are explicit and specific by dispatching actions which are processed by a reduxer that accepts the previous state together with the actions and returns the next state.

### In a React/Redux application where do you make API requests to the backend?

API requests to the backend are made in the actions files. One way is to use Redux Thunk middleware, which creates action creators / thunks: these can return a function instead of an action, which can make asynchronous API calls and can dispatch synchronous actions.
```javascript
export function fetchPosts(subreddit) {
  // Thunk middleware knows how to handle functions.
  // It passes the dispatch method as an argument to the function,
  // thus making it able to dispatch actions itself.
  return function (dispatch) {
    // First dispatch: the app state is updated to inform
    // that the API call is starting.
    dispatch(requestPosts(subreddit))
    // The function called by the thunk middleware can return a value,
    // that is passed on as the return value of the dispatch method.
    // In this case, we return a promise to wait for.
    // This is not required by thunk middleware, but it is convenient for us.
    return fetch(`https://www.reddit.com/r/${subreddit}.json`)
      .then(
        response => response.json(),
        // Do not use catch, because that will also catch
        // any errors in the dispatch and resulting render,
        // causing a loop of 'Unexpected batch number' errors.
        error => console.log('An error occurred.', error)
      )
      .then(json =>
        // Here, we update the app state with the results of the API call.
        dispatch(receivePosts(subreddit, json))
      )
  }
}
```

### What is the difference between a Container and a Component?

Component is part of the React API. A Component is a class or function that describes part of a React UI. Container is an informal term for a React component that is connect-ed to a redux store. Containers receive Redux state updates and dispatch actions, and they usually don't render DOM elements; they delegate rendering to presentational child components.

### What is a Higher Order Component? Put some examples.

- A higher-order component (HOC) is an advanced technique in React for reusing component logic. HOCs are not part of the React API but are a pattern from it’s components and their nature. More concretely, instead of being a normal component that transforms props into UI, it’s a function that takes a component and returns a new component.
- You can imagine that in a large app, this same pattern of subscribing to DataSource and calling setState will occur over and over again. We want an abstraction that allows us to define this logic in a single place and share them across many components. This is where higher-order components excel.
- We can write a function that creates components, like CommentList and BlogPost, that subscribe to DataSource. The function will accept as one of its arguments a child component that receives the subscribed data as a prop. Let’s call the function withSubscription:
```javascript
const CommentListWithSubscription = withSubscription(
  CommentList,
  (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
);
```
The first parameter is the wrapped component. The second parameter retrieves the data we’re interested in, given a DataSource and the current props. When CommentListWithSubscription and BlogPostWithSubscription are rendered, CommentList and BlogPost will be passed a data prop with the most current data retrieved from DataSource:

```javascript
function withSubscription(WrappedComponent, selectData) {
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```

Note that a HOC doesn’t modify the input component, nor does it use inheritance to copy its behavior. Rather, a HOC composes the original component by wrapping it in a container component. A HOC is a pure function with zero side-effects.
The wrapped component receives all the props of the container, along with a new prop, data, which it uses to render its output. The HOC isn’t concerned with how or why the data is used, and the wrapped component isn’t concerned with where the data came from.

### What’s wrong with this code? “this.setState({ score: this.state.score - 1 })”

setState is an asynchronous function, so the local state won’t be immediately updated. Computed state is better off kept in the render function so that it updates when the component re-renders and will have its value updated by that point.

### Why is it that React sometimes requires a key property to be specified?

When React reconciles the keyed children, it will ensure that any child with key will be reordered (instead of clobbered) or destroyed (instead of reused). The key here is to understand not everything in the DOM has a representation in React "Virtual DOM" and, because direct manipulations of the DOM (like a user changing an <input> value or a jQuery plugin listening an element) are unnoticed by React, not using unique and constant keys will end up with React recreating the DOM node of a component when the key is not constant (and losing any untracked state in the node) or reusing a DOM node to render another component when the key is not unique (and tying its state to this other component).

### Explain the Virtual DOM and what React uses it for:

Standard DOM manipulation is typically very slow and inefficient because even if only one item changes, it will update everything. React uses a Virtual DOM, a lightweight representation or copy of each DOM object. Each virtual DOM object has the same properties as a real DOM object but it can’t directly change what’s on the screen. When rendering a JSX element, every single virtual DOM object gets updated (extremely quickly) and a comparison is made between the newly updated virtual DOM with a virtual DOM snapshot that was taken right before the update. This way, React can figure out exactly which virtual DOM objects have changed (process is called ‘diffing’). Once React completes this process, it updates only those objects on the real DOM and ignores anything else that was not updated in the virtual DOM. This is where React’s performance comes from.

### Advantages of Redux
- Predictability of outcome – Since there is always one source of truth, i.e. the store, there is no confusion about how to sync the current state with actions and other parts of the application.
- Maintainability – The code becomes easier to maintain with a predictable outcome and strict structure.
- Server side rendering – You just need to pass the store created on the server, to the client side. This is very useful for initial render and provides a better user experience as it optimizes the application performance.
- Developer tools – From actions to state changes, developers can track everything going on in the application in real time.
- Community and ecosystem – Redux has a huge community behind it which makes it even more captivating to use. A large community of talented individuals contribute to the betterment of the library and develop various applications with it.
- Ease of testing – Redux’s code is mostly functions which are small, pure and isolated. This makes the code testable and independent.
- Organization – Redux is precise about how code should be organized, this makes the code more consistent and easier when a team works with it.

### Roles of Redux's reducers and the store
- Reducers are pure functions which specify how the application’s state changes in response to an ACTION. Reducers work by taking in the previous state and action, and then it returns a new state. It determines what sort of update needs to be done based on the type of the action, and then returns new values. It returns the previous state as it is, if no work needs to be done.
- The store is a JavaScript object which can hold the application’s state and provide a few helper methods to access the state, dispatch actions and register listeners. The entire state/ object tree of an application is saved in a single store. As a result of this, Redux is very simple and predictable. We can pass middleware to the store to handle processing of data as well as to keep a log of various actions that change the state of stores. All the actions return a new state via reducers. Note that in Redux there is only 1 store while in other Flux frameworks there can be multiple stores.

Sources:
https://www.edureka.co/blog/interview-questions/react-interview-questions/,
https://www.toptal.com/react/optimizing-react-performance,
https://coderwall.com/p/jdybeq/the-importance-of-component-keys-in-react-js,
https://reactjs.org/docs/higher-order-components.html,
https://stackoverflow.com/questions/34385243/why-is-immutability-so-important-or-needed-in-javascript
