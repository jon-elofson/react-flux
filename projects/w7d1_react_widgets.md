# React Widgets

## No jQuery
* This entire project is possible without using jQuery *

### Autocomplete widget with list of names
* make an autocomplete component that has an input and an unordered list
* the list of names will be passed in as a property
* the initial state is an empty string
* when a user types something into the input, the state is updated with
  the new value
* in the render method make an `li` inside the `ul` for every name that
  begins with the value in the input box

### Weather Clock
* start by making a simple clock that displays the time
* it should update every second
* the initial state should be the current time (`new Date()`)
* use the `componentDidMount` function to create an interval
* make sure you store the interval's id in a callback so you can cancel
  it later
* we are going to use the [open weather API][weather] to get the current
  weather conditions
* make a new component for the weather
* when the component mounts get the user's location
* use the `navigator.geolocation` API (this won't work locally in
  chrome, try Firefox)
* when the location is received, query the API using a raw
  `XMLHttpRequest`. See [this link][vanilla-ajax] if you need help.
* parse the response, use this to set the state, triggering a re-render
* use your new weather state to display the current temperature

[weather]: http://openweathermap.org/current
[vanilla-ajax]: http://stackoverflow.com/questions/8567114/how-to-make-an-ajax-call-without-jquery

### Tabs

* make a `Tabs` component. It should receive an array of objects with
  `title`s and `content`s as a property.
* its initial state should have a selected tab index of 0
* when a user clicks a header, it should update the selected index
* when the selected index changes and a re-render occurs, display the
  content from the newly selected tab
* I recommend making a second component just for the `Headers`

### Bonus!

* write SNAKE in react! SNAKE!