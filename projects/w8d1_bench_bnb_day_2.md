# BenchBnB Day 2

## Phase 7: React Router
* include [the react router][react-router-source] in your
  `vendor/assets/javascripts` folder and `require` it in `application.js`
* instead of using `React.render` to directly place your `Search`
  component directly into `#content`, set up the router to render
  `Search` as the default route
* create an `App` react component with a `RouteHandler`, mine looked
  like this:

```javascript
  // app/assets/javascripts/bench_bnb.js.jsx
  var root = document.getElementById('content');
  var RouteHandler = ReactRouter.RouteHandler;
  var Route = ReactRouter.Route;
  var App = React.createClass({
    render: function(){
      return (
          <div>
            <header><h1>Bench BnB</h1></header>
            <RouteHandler/>
          </div>
      );
    }
  });
  var routes = (
      <Route name="app" path="/" handler={App}>
        <ReactRouter.DefaultRoute handler={Search}/>
      </Route>
  );
  ReactRouter.run(routes, function (Handler) {
    React.render(<Handler/>, root);
  });
```

* when your app works exactly as before we introduced the router, you
  may move on

## Phase 8: Creating Benches
* create a new React component, `BenchForm`
* it should render a form with fields for `lat`, `lng`, and `description`
* add a new column to the `benches` table, `seating`
* this new column will store how many people can sit together on the
  bench at the same time
* create a new route, `/new`, for this new `BenchForm` component
* test the route by navigating to `#/new`, the map should disappear
* add a `createBench` function to the `ApiUtil`. It should make a `POST`
  request and then add the newly created bench using a new action that
  you must create
* submission of the form should run `ApiUtil.createBench` confirm this
  by using the form to create a bench
* in the `Map` component, add an event handler for a `click` of the
  Google Map. When the `click` occurs navigate to the `new` route. Include the clicked
  coordinates in a query string and then use these values to populate the
  form fields.
* when the form is submitted navigate back to the default route

## Phase 9: Filtering By Seating
* create a new React Component, this will be used to filter benches by
  seating
* this should be a child of the Search view
* when this changes, just like the map, it should cause a fetch with
  params
* these params should also include, now, the requested number of seats
* move on when you can filter by number of seats

## Phase 10: Show Page
* when the bench is clicked on in the index, you will now show the Show
  page
* clicking a marker on the map search view should also navigate to the
  show page
* this is a page dedicated to a single bench
* create a new route for this page
* this view should also have a map with the single bench on it

## Phase 11: Pictures!
* when you create a new bench, allow a user to also add a photo using
  Filepicker!
* you will need to create some new columns in your pictures table
* display these pictures on both the show page and the index


## Phase 12: Reviews
* on the show page, a user should be able to add reviews
* this should be text as well as a score from 1 to 5
* display the score as a list of star images!
* in the index, show the average score for each bench

## Bonus
* every bench can have multiple photos
* show page should have a carousel

[google-map-doc]: https://developers.google.com/maps/documentation/javascript/tutorial
[event-doc]: https://developers.google.com/maps/documentation/javascript/events#MarkerEvents
[map-markers]: https://developers.google.com/maps/documentation/javascript/markers
[lat-lng-docs]: https://developers.google.com/maps/documentation/javascript/reference#LatLngBounds
[react-router-source]: https://raw.githubusercontent.com/rackt/react-router/master/lib/umd/ReactRouter.min.js