# React.js

### What is React?

A JavaScript library for creating UI components. React's specialty is
exactly what its name implies: reaction. When the data that is
represented by the UI changes, React *reacts* by updating the UI.


## What isn't React?

A front end MVC framework. There are no models, no collections, no
`collection.fetch()`, no controllers, no templates, and no routers.

### Why would we need React?

All that `this.listenTo(this.model, "change:title", this.render)`
business was a pain, right? React's mission is to manage *all* UI
updates when any piece changes. That means that all the painful work of
figuring out which subview to re-render/change using jQuery will become
obsolete. Interested yet?

It gets better: the other primary focus of react is to provide a
*simple* interface for front end developers to use. You describe how you
want your UI to look at any given point in time and React takes care 
of the rest.

### How does it work?

In traditional JavaScript applications, you must look at which pieces of
data changed and surgically update the DOM to reflect the new state. In
React, when the component is initially rendered, the markup is generated
and added to the document. When the data is changed, React renders
again, but instead of replacing the component wholesale, it updates only
the bit that has changed. It effectively runs a 'diff' on what is there
compared to what should be there. This way the absolute minimum change
can be applied to the DOM. This process is called *reconciliation*.

This process of *reconciliation* is so fast that we don't need to
carefully check which parts are rendering. We can re-render an entire
page in milliseconds. 

### Where does React come from?
Facebook. Instagram.