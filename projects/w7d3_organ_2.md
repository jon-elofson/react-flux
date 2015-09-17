# Flux Organ Project (Part 2)

## Tracks

### Front End

* Before we worry about how we want to persist a track on the server,
  let's get track recording working on the front end.

#### `util/track.js`

* We'll want a `Track` class to represent a recorded song.
* Write a `Track` constructor function that accepts an attributes hash
  as its argument. The hash will contain a `name` and an optional
  `roll`.
* The `roll` will be an array containing instructions for playing the
  track. As we record the track, the `roll` will have to be updated
  whenever there is a change to the notes being played.
  * When the notes change, we'll want to `push` the updated notes, along
    with a `timeDelta` (the time that has passed since we started
    recording).
* Write instance methods to start and stop recording.
* Write an `addNotes` instance method. This should take an array of
  notes and add them to the `roll`, together with the current
  `timeDelta`.

#### `components/recorder.js`

* Make a new `Recorder` React component. Your component should have
  buttons for recording and playing tracks.
* When the component mounts, it should create a new instance of `Track`.
* When the record button is clicked, call your Track object's
  `startRecording` method.
* The `Recorder` component should listen for changes in the `KeyStore`.
  When the store changes, call your Track's `addNotes` method, passing
  in the new notes.
* When record is pressed again, the track should stop recording.
* Once we've recorded a track, we should be presented with a button
  allowing us to save the track to the database.
* We should also be able to play the track back. Write a `Track#play`
  method to accomplish this.
* Don't move on until you're able to record and play back tracks!

#### `TrackStore`

* Make a store to hold the tracks.
* Write actions so that tracks can be added to the store.
* When a track is saved from the `Recorder`, it should end up in the
  store

### `Jukebox` and `TrackPlayer`

* Make a `Jukebox` React component.
* The Jukebox should display all the tracks in the `TrackStore`.
* Make a new component, `TrackPlayer`. This component will be nested
  inside of your `Jukebox`, and it will represent an individual track.
  It should have buttons to **play** and **delete**.
* Once you're able to view and play tracks in your Jukebox, you're ready
  to move on!

### Back End

It's finally time to start saving songs to the server. Write a Rails
Track model, as well as a TracksController. The Track model should have
columns for `name` (a string), and `roll` (a JSON object). SQLite3
doesn't support JSON columns natively; you could handle the conversions
yourself, but it's easier to simply use Postgres.

In your TracksController, write actions for `create`, `index`, and
`destroy`. The `index` action should render a JSON representation of
each saved Track object.

To wire everything up, we're going to need a few more actions and
utilities. Write a utility to fetch, save, and destroy tracks through
your JSON API, along with any other tools that might be necessary.

## Extra Awesome Features

* **Looping**: add a setting to allow tracks to play continuously.
* **Jam Session**: allow users to play on the organ while a track runs
  in the background.
* **Scales**: pentatonic, minor, etc.
* **Playlists**: queue up tracks to be played one after another.
* Options to change volume/waveform of notes and tracks
* Export tracks as audio files so they can be saved to the client's
  machine
