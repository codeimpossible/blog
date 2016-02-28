---
title: Why I dislike the es6 arrow
date: 2016-02-02 23:13:01
tags:
- development
- javascript
- es6
---

Yeap, I'm not a fan of the new [arrow function syntax introduced in es6/2015](http://babeljs.io/docs/learn-es2015/#arrows-and-lexical-this). In case you haven't used it yet, here's a quick and dirty intro:

```javascript
var times_2 = [1,2,3,4].map((num) => num * 2);

times_2.forEach((num) => {
  console.log(num);
});
```

At first, this looks like a great shortcut. Hell, I was super excited when I first saw the arrow function proposal. It reminded me of the lambda syntax from my other favorite language, C#. Buuuut it's not like the lambda syntax from C#. Not really.

The ES6 arrow function creates a "lexical scope" when it is used. Lexical (or "static") scoped functions are functions where the `this` context is set to it's parent, and cannot be altered. This means that the examples above would translate (roughly) to this JavaScript:

```javascript
var times_2 = [1,2,3,4].map(function(num) {
  return num * 2
}.bind(this));

times_2.forEach(function(num) {
  console.log(num);
}.bind(this));
```

Now in these two examples the scoping isn't an issue. In fact in a lot of cases you won't care about the `=>` operator creating a static scope. But it's really easy to forget, especially when everyone on the internet just refers to this thing as the "ES6 arrow function".

To me, the better name for this is the "static scope operator". Glossing over the fact that this operator creates a static scope seems like a bad idea to me, since it obscures what the behavior is. Scope is an incredibly tricky thing to understand in JavaScript, so to me it should be very obvious when we are about to muck around with it.

This obfuscation of the static scope operators intent often leads to code that can be misinterpreted or hard(er) to understand. What if we wanted to create an action handler for a React component?

```javascript
class MyComponent extends React.Component {
  onLinkClick = () => {
    // do stuff
  }

  render() {
    return <a onClick={this.onLinkClick}>Clicky</a>;
  }
}
```

The few lines of code here is a plus, but that `onLinkClick` property is hard to grok at first glance. When we write code, we're writing code for the people that we work with now and in the future who will maintain our code. Changing `render()` to use `.bind` could clean this up, but we could also use lodash to bind multiple functions to the current component instance:

```javascript
import _ from 'lodash';

class MyComponent extends React.Component {
  constructor() {
    _.bindAll(this, 'onLinkClick');
  }

  onLinkClick() {
    // do stuff
  }

  render() {
    return <a onClick={this.onLinkClick}>Clicky</a>;
  }
}
```

To me the second example is much clearer about what the code is doing. Some will argue that the use of a third-party lib is just bloat, but then you could also write your own helper method to do this. My point is to think more about the abstractions you use day-to-day and to ask yourself if they are improving the readability and function of your code, or if they are just saving you a few keystrokes.
