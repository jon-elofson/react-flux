# Flux Organ Project (Part I)

## Overview

In this project we will be making a simple musical organ that can be
played using the keys on your keyboard! When we are finished a user will
be able to save their track and play it back. We will implement this
using React.js and Flux. Hitting a key will create an action that adds a
key to the `KeyStore`. Changes to the `KeyStore` will cause your React
components to update themselves.

## Phase 1: Flux Structure

* Add the following to your Gemfile:

```rb
gem 'react-rails', '~> 1.0'
gem 'flux-rails-assets'
```

* Make a `StaticPagesController`. Give it a `root` action that renders a
  view containing a `div` with id `content`. Update your router to make
  sure this view renders on your site's root URL.
* Next, structure your app's `assets/javascripts` directory. We're going
  to organize our files like this:

```
assets
  |
  + ...
  + javascripts
    |
    + actions
    + components
    + constants
    + dispatcher
    + stores
    + util
    application.js
    organ_grinder.js.jsx
  + ...
```

* Add the following lines to your `application.js`:

```
  //= require flux
  //= require eventemitter
```

* Finally, create the dispatcher file:

```js
//dispatcher/dispatcher.js
AppDispatcher = new FluxDispatcher();
```

## Phase 2: Notes

* Make a `notes.js` file inside your `util` folder.
* Write an IIFE to wrap all the code in this file.
* In order to make beautiful music, we'll need a pointer to the web
  audio context. Declare this as a local variable:

```javascript
var ctx = new (window.AudioContext || window.webkitAudioContext)();
```

* Through the miracle of closures, this audio context will be available
  to the rest of our code.
* We need to have some way of manipulating this context to create,
  destroy, and modify sounds. Let's create a `Note` class for this
  purpose. Declare your class in the global (`window`) namespace.
* Your `Note` constructor should accept as its sole argument a
  `frequency`, measured in Hertz (Hz)
* The next couple steps may seem magical. It's not important for you to
  have a deep understanding of the browser audio API, so we're providing
  some code to get you up and running. Just play along, and try not to
  let this bother you!
* In the constructor, set up instance variables for an **oscillator
  node** and a **gain node**. Let this snippet guide you:

```js
var createOscillator = function(freq){
  var osc = ctx.createOscillator();
  osc.type = "sine";
  osc.frequency.value = freq;
  osc.detune.value = 0;
  osc.start(ctx.currentTime);
  return osc;
};

var createGainNode = function(){
  var gainNode = ctx.createGain();
  gainNode.gain.value = 0;
  gainNode.connect(ctx.destination);
  return gainNode;
};
```

* Connect the gain node to the oscillator node:

```js
this.oscillatorNode.connect(this.gainNode);
```

* Write a `Note#start` method.
* This method should set `this.gainNode.gain.value` to `0.3`. Sorry for
  the magic number, but it's just the right value! I can't explain it!
* Write a `Note#stop` method that sets it back to `0`.
* Once you have your `Note` class set up, make a new `Note` instance in
  the console and call `start` on it. It should actually make noise! I
  recommend a frequency between 500 and 1000, those seem to sound best.

### `Tones`

* Create a `util/tones.js` file.
* In this file, write a `window.NOTES` constant; this will just be
  a JavaScript object mapping note names to frequencies.
* Feel free to use [this table][note-frequencies] as a resource.

[note-frequencies]: http://www.phy.mtu.edu/~suits/notefreqs.html

## `KeyStore`

* Make a `KeyStore`. This should keep track of all the keys currently
  being pressed down by the user.

* Make a `util/key_listener.js` file. This code should create an action
  when a key is pressed and create another action when a key is
  released. For example, the `KeyListener` might add `C5` to the
  `KeyStore` when the `a` key is pressed.

## React Components

### Key

* Make a `Key` component. This will be the visual representation for a
  single note in your organ. This component will be passed a single
  `noteName` as a prop. When the component mounts, create a `Note`
  instance for that `noteName`;store this as an instance variable.
* The `Key` should listen to the `KeyStore`. If the `noteName` for the
  `Key` is in the `KeyStore`, the `Key` should `start` its `Note`.

### Organ

* Make a new React component, `Organ`. This should render a `Key` for
  each of the `window.TONES`.
* At this point, you should be able to make beautiful music just by
  typing on your keyboard!
* Using CSS rules and the `state` of your `Key` component, update the
  key visually when its note is being played.

## Bonus

* It's a keyboard; have fun! Add more notes!
* Make additional keys that play entire CHORDS.
* Make it beautiful. This could be a *key component* of your portfolio
  (see what we did there?)
* Implement other octaves and waveforms.

## Part 2: Saving Tracks

Find the instructions [here][organ-grinder-2]

[organ-grinder-2]: w7d3_organ_2.md
