# Todo React Application

### Phase 0: Rails Back End
* create a `todos` table and a `Todo` model with the following attrs:

```ruby
create_table "todos", force: :cascade do |t|
  t.string   "title",      null: false
  t.string   "body"
  t.boolean  "done",       null: false
  t.datetime "created_at", null: false
  t.datetime "updated_at", null: false
end
```

* create a rails JSON API with the following routes:
```ruby
GET /todos/ #index
GET /todos/:id #show
POST /todos/ #create
DELETE /todos/:id #destroy
PATCH /todos/:id #update
```

* follow the instructions in [this document][react-rails] to get React

### Phase 1: Modeling Todos on Front End
* Make a new JavaScript file, `app/assets/javascripts/todos.js`, this will be home
  to all the code that interacts with our Rails API Todos resource.
* Write a constructor function, it should take a single callback as
  an argument. Store this callback as an instance variable. We will call
  this callback every time our _collection_ changes, so call it
  `this.changed`.
* The constructor function should store a `this._todos` array instance
  variable. This will hold the todo JavaScript objects once we get them
  from the server.
* On the `prototype` of your constructor function, write a `fetch`
  method. This should make a get request to `/api/todos/`. Upon success,
  set `this._todos` to the array of objects returned from the server and
  call `this.changed`.
* Write a `create` method. This should take an object as an argument and
  make a `POST` request to create the instance in the database. Upon
  success add the server response (which should contain an `id`) to the
  `this._todos` array. Don't forget to call `this.changed` upon success.
* Write a destroy method. Upon success be sure to `splice` the object
  out of `this._todos` before calling `this.changed`.
* Finally, add a `toggleDone` function. This should make a `PATCH`
  request and change the specified `todo`'s `done` to the opposite of
  whatever it is. Then, of course, call `this.changed`.
* Write an `all` method, this should return ALL of the `todos`.
* Make a `StaticPagesController` and give it a `root` action. Set your
  app's `root to:` to the root action. It should just be a blank page
  for now. 
* Test out all the functionality in the console to ensure it works.

### Phase 2: Displaying Todos with React.js
* Start by creating a `todos.js.jsx` file. This file will be compiled
  into non JSX enriched pure JavaScript. `<Todolist {todos}/>` will
  `React.createElement(Todolist, {todos: todos})`.
* Our first goal is to simply display the titles for our todos. Make a
  `TodoList` react component that takes an instance of `todos` as a
  property.
* Make a function that will call `React.render` and pass a `TodoList` 
  element as the first argument. Store this function in a local
  variable called `globalRender`. The `globalRender` function will be 
  the callback we pass in to the constructor function that creates the 
  Todos collection.
* This React component's `render` method should return a `div` with an
  array of `li`s that simply have the title from each of the todos.
* We don't, of course, have any todos to display yet. We have an empty
  instance of our collection class from `todos.js`. In the
  `componentDidMount` life cycle function, call `fetch` on your todos
  `prop`. Because we passed `globalRender` as the callback
  to the todos collection, `fetch` should cause a global re-render to be
  triggered automatically. Move on when you can see your Todos.
* Lets make individual components for each of the todo items. Make a
  `TodoListItem` React component. It should receive a single todo item
  as a property. Its `render` function should render a `div` at the
  root that contains child `div`s for the title and body.
* Change the render method of `TodoList` to create `TodoListItem`s
  instead of `li`s.

### Phase 3: Making New Todos
* Create another component: `TodoForm`. This should `render` a form with
  a field for `title` and `body`. 
* Make this a [controlled component][react-forms]. The initial state
  should be `{title: "", body: ""}`. `onChange` for the `title` or
  `body`, update the `state`. 
* `onSubmit` of the form, you should use the todos collection's 
  `create` function from phase 0.

### Phase 4: Deleting Todos
* Add a destroy button to the `TodoListItem` component.
* `onClick` it should destroy the todo item by calling `destroy` on the
  `todos` collection. This should automatically trigger a
  `globalRender`.

### Phase 5: Marking Done
* Make yet another React component. Call this one `DoneButton`.
* `render` should return a `button` that has the text `done` when the
  todo is not yet complete and `undo` when the todo is complete.
* `onClick` of this button, have the component call `toggleDone` on the
  `Todos` collection.
* Add a `DoneButton` to the `TodoListItem` view.

### Phase 6: Collapsible Todos
* Lets create one more React component, `TodoDetailView`. This component
  will contain the `body` as well as the delete button for the todo.
* This new component will be a child of `TodoListItem`. Initially, the
  detail view is not shown, so only the title and done button are
  visible. When the todo is clicked, the detail view is displayed. Using
  `this.state` and `render` implement this functionality.

### Phase 7: Todo Steps
* todos should now have steps! add a `steps` table and make a controller
  and actions so that todos can have steps
* each step should also have a 'done' button
* write the appropriate front end code to create, destroy, and display
  steps

[react-rails]: ../react_readings/react_on_rails.md
[react-forms]: http://facebook.github.io/react/docs/forms.html