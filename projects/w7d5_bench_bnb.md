# Bench BnB

## Phase 0: Rails Backend
* create a new rails project
* make a `Bench` model with `description`, `lat` and `lng`
* `lat` and `lng` should be of type `float`
* make a JSON api for this resource, it will need `index` and `create`
* make some seed data with real coordinates in SF using
  `seeds.rb`

## Phase 1: Flux Structure
* `gem 'react-rails', '~> 1.0.0'` and `gem 'flux-rails-assets'` to
  the `Gemfile`
* run `rails g react:install` to add the react `require` statements to
  `application.js`
* add the following lines to your `application.js`

```javascript
//= require flux
//= require eventemitter
```
* this will make the `EventEmitter` and `FluxDispatcher` available
* make a `StaticPagesController`, have it serve a root view with a
  `#content` `div`

```
assets
  |
  + ...
  + javascripts
    |
    + actions
    + components // all React components, both views and controller-views
    + constants
    + dispatcher
    + stores
    + util
    application.js
    bench_bnb.js.jsx
  + ...
```
* create the folders such that your assets folder resembles the diagram
  above
* these folders will store all of our front end goodies
* create the dispatcher file as follows:
```javascript
//dispatcher/dispatcher.js
AppDispatcher = new FluxDispatcher();
```
* that's it

## Phase 2: The Bench Store and Web API
#### Bench Store
* we will now start building the pieces necessary for us to display a
  basic benches index using React components
* start by making a BenchStore
* this object should provide access to all the client side benches
* think of it like a global `Backbone` `Collection`
* the following is the most basic example of a store
```javascript
//stores/bench.js
//implement in an IFFE so we can have a private variable, _benches
(function(root){
  var _benches = [];
  root.BenchStore = $.extend({}, EventEmitter.prototype, {
    all: function(){
      //return a shallow copy so consumer cannot mutate original
      return _benches.slice(0);
    }
  }
})(this)
```

* all this store can do is return all the benches it currently contains

#### Api Utility
* next, lets make a utility function to help us populate this store with
  content from the server

```javascript
//util/api_util.js
ApiUtil = {
  fetchBenches: function(){
    //make an api call using AJAX in here
  }
}
```

* the success callback of the AJAX will contain all the bench objects,
  but how do we insert them into our store? Refer to [the diagram][flux-diagram].
* as we can see, the only thing that can change a store is the
  Dispatcher, and the way the Dispatcher interacts with the stores is
  through actions
* the source of this action is the `ApiUtil`, so it makes sense that this
  should be an `ApiAction`.

#### Action

##### Constants
* before we can create an action we need a constant to name the action
* create a new file, `constants/bench_constants.js`
```javascript
BenchConstants = {
  BENCHES_RECEIVED: "BENCHES_RECEIVED",
}
```
* this will be the _name_ of the action

##### Back to the Action
* create a new file, `actions/api_actions.js`
* create an `ApiActions` object and give it a `receiveAll` function
* `receiveAll` should dispatch an object with an `actionType` of
  `BenchConstants.BENCHES_RECEIVED`

```javascript
ApiActions = {
  receiveAll: function(benches){
    AppDispatcher.dispatch({
      actionType: BenchConstants.BENCHES_RECEIVED,
      benches: benches
    });
  }
}
```

##### Putting it all together
* in the success callback of your AJAX call in the `ApiUtil`, we can
  finally use the action we just wrote. Call `ApiActions.receiveAll` and
  pass in the benches you received as an argument. 
* call `BenchStore.all`, it's still empty, right? The Dispatcher fired
  (hopefully) but the store wasn't listening to the Dispatcher.
* tell the `BenchStore` to listen to the dispatcher by doing the
  following
```javascript
//stores/bench.js
(function(root){
  //...
  var resetBenches = function(benches){
    _benches = benches;
  }
  root.BenchStore = $.extend({}, EventEmitter.prototype, {
    //...
    dispatcherID: AppDispatcher.register(function(payload){
      if(payload.actionType === BenchConstants.BENCHES_RECEIVED){
          resetBenches(payload.benches);
      }
    })
  }
})(this)
```

* we have added a new function, `resetBenches`, as well as the
  `dispatcherID` property which registers a callback to be triggered
  whenever the dispatcher receives an action
* as we can see, if the action is of a type that is relevant to this
  store, we can refresh our internal array of benches with the new data
* finally we should be able to test this thing. In the console, call
  `ApiUtil.fetchBenches()`. Then, `BenchStore.all()`. If it returns an
  array of all the benches in the database we have won.
* make sure that everything is working before moving on

![the story so far](api_store_diagram.png)

## Phase 3: Our First React Component
#### Emitting Events from the Store
* return to our store, when the contents of the `BenchStore` change,
  we need to inform all interested parties by emitting a `CHANGE_EVENT`
* we should add functions for subscribing (and unsubscribing) to this event to our store
* finally we should `emit` this event when our store's contents change
```javascript
//stores/bench.js
(function(root){
  var CHANGE_EVENT = "change";
  //...
  root.BenchStore = $.extend({}, EventEmitter.prototype, {
    //...
    addChangeListener: function(callback){
      this.on(CHANGE_EVENT, callback);
    },
    removeChangeListener: function(callback){
      this.removeListener(CHANGE_EVENT, callback);
    },
    dispatcherID: AppDispatcher.register(function(payload){
      if(payload.actionType === BenchConstants.BENCHES_RECEIVED){
        BenchStore.emit(CHANGE_EVENT);
        resetBenches(payload.benches);
      }
    })
  }
})(this)
```

#### React Component
* make an `Index` React component
* in `bench_bnb.js.jsx`, add a `React.render` call that creates the
  `Index` and places it into the `#content` div
* give it an initial state of `{ benches: Benchstore.all() }`
* when the component mounts, we need to register a listener on the
  `BenchStore` using it's new `addChangeListener` function. We should
  also use your `ApiUtil` function to `fetchBenches`.
* when the store changes, update the state

```javascript
this.setState({benches: BenchStore.all()});
```
* the API call updates the store, which should cause the store to emit an
  event, which should cause the view's state to update which would cause
  a re-render!

![complete flux loop](complete_flux_loop.png)

## Phase 4: The Map
* create a new React component, `Map`
* its `render` function should return a `div` with class `map` and `ref`
  `google-map`
* in the `css` file, make sure to set the `width` and `height` of the
  `.map` to `500px`
* read [the google maps documentation][google-map-doc]
* get a new API key for a JavaScript Google Map
* add a script tag including your API key to your `application.html.erb`
* create a new React component, `Search`
* `Search` should render a `div` with a `Map` and `Index` as children
* in `bench_bnb.js.jsx` render a `Search` component instead of `Index`
  at the root. This should cause the `Map` to be rendered into the page
* when the `Map` component mounts, instantiate the map as follows
* we will just use the default location of San Francisco instead of
  trying to geolocate

```javascript
    //components/map.js.jsx
    //...
    componentDidMount: function(){
      var map = React.findDOMNode(this.refs.map);
      var mapOptions = {
        center: {lat: 37.7758, lng: -122.435},
        zoom: 13 
      };
      this.map = new google.maps.Map(map, mapOptions);
    },
```

* this should cause a genuine Google Map to be rendered to the screen

## Phase 5: Markers on the Map
* when the `Map` component is mounted, register an event listener on
  change of the `BenchStore`
* read about [map markers here][map-markers]
* when the event occurs, create markers for every bench in the array
* move on when all your benches are represented by markers on the map
* one last change: since it doesn't make sense to fetch any markers from
  the API until we know where the map is, move the
  `ApiUtil.fetchBenches` from the `Index` to the idle event of the map
   [read this documentation][event-doc] to learn about Google Map events
* if everything still works, move on to the next phase

## Phase 6: Filtering by Map Location

* when the map idles, we are going to use its current bounds to request
  only benches within the map bounds
* first, let's prepare the back end to search by bounds

### Back End Prep

* I wrote an `Bench.in_bounds` method that returned all the benches that
  were within the boundaries specified by the argument.

```ruby
#...
  def self.in_bounds(bounds)
  # bounds in the following format:
  # {
  #   "northEast"=> {"lat"=>"37.80971", "lng"=>"-122.39208"}, 
  #   "southWest"=> {"lat"=>"37.74187", "lng"=>"-122.47791"}
  # } 
  #... query logic goes here
  end
#...
```

* in the controller, we can expect that these boundaries will be passed
  in as a query string and therefore available in the `params` hash
* instead of rendering `Bench.all` we can instead use
  `Bench.in_bounds(params[:bounds])`

### Sending the Correct Params
* so now our back end is expecting a `GET` request to the `index`
  including a query string, so we must now furnish this request
* return to our map `idle` event handler. Call `getBounds` on the map
  instance to get a `LatLngBounds` instance. Call `getNorthEast` and
  `getSouthWest` to get these coordinate pairs. Get their latitude and
  longitude and format these coordinates into exactly the format our back
  end is expecting. Check [this documentation][lat-lng-docs] for more
  info.
* now, armed with an object containing the current bounds of the map, we
  can use this object in our AJAX call
* pass this bounds object we have created as an argument to
  `ApiUtil.fetchBenches`. Inside `fetchBenches` we can now pass this
  argument to our AJAX call so it can be used as a query string
* verify that when the map moves the correct request including the query
  string is being sent to the server, and the server is responding with
  the correct benches. The `Index` React component should be correctly
  updating with `benches` being added and removed as the map is dragged

### Adding and Removing Markers
* so the map move causes the store to emit a change event, but the map
  needs special treatment. When we detect that the contents of the store
  have changed, we should find out if any markers need to be added or
  removed.
* when a change event occurs, figure out which benches in the store have
  markers representing them on the map. Those without markers should
  have markers created and added. Markers for benches NOT in the store
  should be removed.
* this is expected to be a somewhat difficult problem
* when the only markers on the map are those appearing in the index, you
  may move on

![map bounds diagram](map_idle_diagram.png)

## BONUS!
* when you hover over an index item it should highlight the marker on
  the map. This should require creating a new store.

* look at your app, then look at the real AirBNB. See any differences?
  Give your project a makeover and add a ton of CSS so that it resembles
  the REAL AirBNB.

