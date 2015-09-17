# Components with JSX
### Getting the Library
JSX is not natively understood by web browsers. That means that we must
either compile it into JavaScript on the back end or install some sort
of transformer so that the browser is able to interpret the alien
syntax. In another reading we will discuss compiling the JSX, but to
instead use the transformer, include the following in your `head`.
```html
<script src="https://fb.me/react-0.13.3.js"></script>
<script src="https://fb.me/JSXTransformer-0.13.3.js"></script>
```
Then write your code which contains JSX inside of `text/jsx` type script
tags instead of `text/javascript`.

### The Cool Stuff
JSX provides an alternate syntax to `React.createElement` which is
frankly a huge improvement. It allows a front end developer to
expressively describe how the component will look when rendered using
syntax almost exactly like html. 

Our previous example created a button that counted clicks using the
following snippet:

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

We created a nested tree structure using `React.createElement`. We can
build a tree using the third argument of the function, which takes the
children of the element currently being created.

If we used JSX instead to create this tree of React elements, our code
would look like this:

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
      <div>
        <button onClick={this.click}>CLICK ME</button>
        <span>{this.state.count}</span>
      </div>
    );
  } 
});
```

Isn't that a sight to behold? It is now incredibly obvious what elements
will be created when this component is rendered into the DOM. 

Previously, we gave properties to the elements we were creating by
passing an object as the second argument to `React.createElement`. With
JSX we pass properties as though they were HTML attributes like `id` or
`class`. This even applies to events!

### Weirdnesses

Creating the component class `ClickCounter` seemed incredibly easy and
intuitive with JSX. There must be some downsides, right?

```javascript
React.render(
    <ClickCounter/>,
    document.getElementById('my-component')
);
```

Have a look at the render method, using JSX. `ClickCounter` is not an
HTML tag name. But here we are pretending it is. What's up with that?
Well, JSX is really just a syntax for expressing nested data structures.
React doesn't traffic in _real_ HTML elements, but it does keep track of
the tree structure and each node's properties. `<ClickCounter>`
expresses that an instance of `ClickCounter` should be created and
rendered with no properties or children. This is just syntactic sugar
for `React.create(ClickCounter, {})`.

Weirdness number two is that, as mentioned, when we pass properties to
an element being created, we do so as if they were simply attributes.
This is very intuitive for things like `id`, but what about `class`?
Well since it is a reserved word, when passing a `class` attribute, we
must instead use `className`.