---
title: Why I dislike the es6 arrow
date: 2016-02-02 23:13:01
tags:
- development
- javascript
- es6
---

When I first read that javascript was getting a arrow function shortcut, I was happy. Like this kind of happy:

<img src="http://www.reactiongifs.com/r/yay2.gif" />

Seriously. So happy. Check out the code below and tell me it doesn't feel great.

```javascript
var times_2 = [1,2,3,4].map((num) => num * 2);

times_2.forEach((num) => {
  console.log(num);
});
```

Pretty, great, right? The arrow function shortcut seemed like a perfect port of the lambda syntax from C#. Buuuut it's not. Like, not at all.

When you use the ES6 arrow function, it creates a lexical scope. Lexical (also called static) scoped functions are functions where the `this` context is set to it's parent, and cannot be altered. This means that the examples above would translate (roughly) to this JavaScript:

```javascript
var times_2 = [1,2,3,4].map(function(num) {
  return num * 2
}.bind(this));

times_2.forEach(function(num) {
  console.log(num);
}.bind(this));
```

Now in these two examples the scoping isn't an issue. In fact in a lot of cases you won't care about the `=>` operator creating a static scope. But it's really easy to forget, especially when everyone on the internet just refers to this thing as the "ES6 arrow function".

For real? "Arrow function" makes it sound like this isn't doing anything other than dropping a function into my code. It's a bad name and it should feel bad.

<img src="http://i.imgur.com/fLlgFSC.gif" />

To me, the better name for this is the "static scope operator". Yeah it's longer, fite me. _Authors Note: don't really fight me, I'm overly squishy and bruise easily. Let's grab a drink of your preference and talk about movies or comics instead_. 

Using a name that doesn't call out that this operator creates a static scope seems like a bad idea to me. Scope is an incredibly tricky thing to understand in JavaScript, so to me it should be very obvious when we are about to muck around with it.

This obfuscation of the static scope operators intent often leads to code that is harder to understand. For example, what if we wanted to create an action handler for a React component?

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

The low number of lines of code here is a plus, but that `onLinkClick` property is hard to grok at first glance. When we write code, we're writing code for the people that we work with now and in the future who will maintain our code. Code is for coworkers. 

Let's try cleaning this up.

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

To me this second example is much easier to read and is clearer about how the code functions. The scope binding is clear, and explicit.

So when do I use a static scope operator? Usually in simple callbacks, like map/reduce or a one line promise handler. I don't see the value of using `=>` for every function by default, especially not in the class definition like above. But if you disagree, cool. I'd love to hear your points so post a comment, twit me on twitter (@codeimpossible), or chat with me at a conference.

My intention here wasn't to make you feel bad for using static scope operators (yep, I'm going to keep calling it that). I do hope that this post made you think more about the abstractions you use and make you ask yourself if they making your code easier to read, maintain, and test, or if they are just saving you a few keystrokes.
