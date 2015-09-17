# My First Component

### Getting the library from a cdn

Start by requiring the react library using the following script tag:
```javascript
<script src="https://fb.me/react-0.13.3.js"></script>
```
Put this in the document `head`. Now, every time we load this page,
before evaluating the body, the browser will download the file pointed
to by the script tag. Therefore: we should be able to use the react
components immediately in the body. 

### Make a space for it in the body

In the `body`, make an empty div and give it the `id` of `my-component`.
All of our React stuff will go into this div. We give it an id so that
we can easily find it later. 

### Render with React

Make a script tag beneath your empty div. Inside the script tag add the
following:

```javascript
React.render(
    React.createElement('div', {}, 'Hello from React!'),
    document.getElementById('my-component')
);
```

Refresh your browser, you should see the div where it belongs! 

### What's going on?

We are using two of React's core methods:
[`createElement`][createElement-doc] and
[`render`][render-doc].
The `document.getElementId` bit returns a reference to the DOM node we
created. Did you know you can get these without jQuery?

Please read both links in the paragraph above. See? React isn't so
scary!

When we `React.render` something, it creates a version in React's
virtual DOM. Then, React compares its virtual DOM with the real DOM and
makes incredibly quick and efficient changes. When we `render` in
backbone, usually we are blowing up DOM elements and creating new ones.
React does a much faster job with its 'virtual DOM'.

The `createElement` method creates an instance of a React element. This
is NOT a DOM element. It is closer to a POJO than a DOM node, so don't
even think about trying to jQuery it into the DOM. The first argument is
the type of element/component that React will create. This can be either
a string, like `'div'` or a reference to a component created using
`React.createClass`. The second argument to `createElement` is the
properties that will be passed to that component. As an argument,
`{className: 'my-div'}` will give the component a `class` of `my-div`
when it is rendered in to the DOM. We can also use props to set event
handlers. `{onClick: myCallback}`, will cause `myCallback` to be
triggered when a click event occurs.

`createElement` also takes an optional third argument. This is the
_children_ of the component. It can be empty, or contain other React
components, or strings.

[render-doc]: http://facebook.github.io/react/docs/top-level-api.html#react.render
[createElement-doc]: http://facebook.github.io/react/docs/top-level-api.html#react.createelement

### Take it to the next level

In this section, let's make a more interactive component. Let's put a
button into the div. When the button is clicked, let's increment a
counter. We will put the value of that counter into a `span`.

Since we are describing a component that has behavior and state it makes
sense to use a class, right? Indeed. React provides a helpful
[`createClass`][create-class] method for this purpose. Think of the 
_class_ created by React as a Backbone view and template all rolled 
into one; it has similar functionality.

```javascript
var ClickCounter = React.createClass({
  render: function(){
    return (
    React.createElement('div', {}, 
      React.createElement(
        'button', 
        {},
        "CLICK ME"
      )
    )
   );
  } 
});
```

So here is an early version of our fancy new React component. All it
contains is a `div` holding a `button` html element. The button element
is passed no properties. 

On its own, this component will not make its way into React's virtual
DOM or the actual DOM. We need to use `React.render` to get it in.

```javascript
React.render(
    React.createElement(ClickCounter, {}, ""),
    document.getElementById('my-component')
);
```

The snippet above is what we need to get the button into the DOMs. As we
discussed above, the first argument to `createElement` can be either a
string with an html tag name or an existing React `class`. Try this and
make sure it works.

[create-class]: http://facebook.github.io/react/docs/top-level-api.html#react.createclass

### Giving our component state

So we want this component to keep track of how many times the button has
been clicked. Therefore, the initial state should be a count of 0. We
can write a `getInitialState` method to define this.

```javascript
var ClickCounter = React.createClass({
  getInitialState: function(){
    return {count: 0};
  },
  render: function(){
    return (
    React.createElement('div', {}, 
      React.createElement(
        'button', 
        {},
        "CLICK ME"
      ),
      React.createElement('span', {}, this.state.count)
    )
   );
  } 
});
```

`getInitialState` seen above defines the initial state. This is used in
the render method. The object returned from `getInitialState` will
become the `this.state` object. We can modify this value throughout the
life cycle of the component. 

### Clicky clicky

Finally we need to set up a `click` event that will update the
component's state and induce a re-render. To install a click event 
handler on a component, give it an `onClick` property in the `render` 
method. The value for `onClick` should be a function.

When the button is clicked we can use 
`this.setState({state: this.state.count + 1})` to update the state. This
has the side effect of *automatically* re-rendering the component. So,
we should just see it update before our very eyes!

```javascript
var ClickCounter = React.createClass({
  getInitialState: function(){
    return {count: 0};
  },
  click: function(event){
    event.preventDefault();
    this.setState({count: this.state.count + 1});
  },
  render: function(){
    return (
    React.createElement('div', {}, 
      React.createElement(
        'button', 
        { onClick: this.click },
        "CLICK ME"
      ),
      React.createElement('span', {}, this.state.count)
    )
   );
  } 
});
```

Now you have made your very first interactive component, wasn't that
fun?
