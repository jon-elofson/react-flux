# React On Rails

Using React with your rails project is surprisingly easy. As you might
expect there exists a great gem that will happily provide React for you,
`react-rails`. 

The first step is of course including the gem in your `Gemfile`. Use the
following line to ensure the latest stable(at the time of writing) version.

```ruby
gem 'react-rails', '~> 1.0'
```

Next, we just need to run the installation script.

```
rails g react:install
```

Now, if we intend to write JSX (and why wouldn't we) we just need to end
our JavaScript files in JSX, so the asset pipeline knows to compile
those files into vanilla JavaScript. (We wont be doing any client side
JSX transformation.)

Finally we should be able to start writing React components using JSX. Go
ahead and create a new `.js.jsx` file and start writing your own React
components.