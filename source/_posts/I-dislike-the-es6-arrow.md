---
title: I dislike the es6 arrow
date: 2016-02-02 23:13:01
tags:
- development
- javascript
- es6
---

Yeap, I'm not a fan of the new [arrow function syntax introduced in es6/2015](http://babeljs.io/docs/learn-es2015/#arrows-and-lexical-this). In case you haven't used it yet, here it is:

```javascript
var times_2 = [1,2,3,4].map((num) => num * 2);

times_2.forEach((num) => {
  console.log(num);
});
```

At first, this looks like a great shortcut. I was super excited when I first saw that javascript was getting a function shortcut similar to the lambda syntax in my other favorite language, C#. However, it comes with what - is to me at least - a pretty unfortunate cost. Static scope.

Static scope is a fancy way of saying that the scope of a function is unchangeable. It's also called lexical scope. Most of the time static scoping is something you won't even care about. It won't negatively affect the code I wrote in the sample above.

But what about when it _does_ affect your code? Will it be obvious enough that `=>` is causing odd scope behaviors? The answer is: it will if you remember that the `=>` operator isn't just a function operator, **it's a static scope operator**.

## Naming things is hard
Which brings me to the first thing I dislike about the "arrow functions" in es6: The name. It should be called the "static scope operator", but everyone refers to them by the "arrow function" moniker. This seems like a bad idea to me, since it obscures what the behavior is. Scope is an incredibly tricky thing to understand in JavaScript, which to me means it should be very obvious when we are about to muck around with it. Saying "this is a nifty new shortcut to make all your functions" isn't the same as "this creates a function with a static scope".

## Scope errors are hard
I mentioned the second thing I don't like about the static scope operator (yes I'm calling it that from now on. come at me): scope errors aren't super obvious when they happen. Which makes things that "should just work" completely fail. Take a look at the following mocha tests:

<a class="jsbin-embed" href="http://jsbin.com/tonusu/2/embed?js">ES6 Static Scope Operator on jsbin.com</a>
<script src="http://static.jsbin.com/js/embed.min.js?3.35.9"></script>

These tests don't work. Seriously. Open the output tab. The `beforeEach` fails because `this` is undefined (_[Why it is undefined is another story, check out this article by Jesús Gollonet](http://blog.jesusgollonet.com/2015/04/21/this-undefined-es6-arrow-functions/)_). If you didn't know (or forgot, like I do all the time) that `=>` creates a static scope then this kind of bug can waste a ton of your time.

## Code complexity is hard
And the third and final thing that I don't like about the static scope operator is that it often leads to code hoop-jumping where you still end up with - again, to me - less readable code. Let's take a look at a react component:

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

Is that really better than this?

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

To me the second example is much clearer about what the code is doing. You can argue that the use of a third-party lib is bad and unclear, but I think a well documented third party lib is better than an obfuscated and confusing operator.

I'll still use static scope operators, but only for very simple code blocks, like the sample in the opening of this post. You're of course free to use what you like, we're grown ups after all, but I would just caution you to think more about the abstractions you use day-to-day and to ask yourself if they are providing more value than the effort required to understand them.
