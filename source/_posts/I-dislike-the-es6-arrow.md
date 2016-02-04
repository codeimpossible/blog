---
title: I dislike the es6 arrow
date: 2016-02-02 23:13:01
tags:
- development
- javascript
- es6
---

Yeap, I'm not a fan of the new [arrow function syntax introduced in es6/2015](). In case you haven't used it yet, here it is:

```javascript
var times_2 = [1,2,3,4].map((num) => num * 2);

times_2.forEach((num) => {
  console.log(num);
});
```

At first, this looks like a great shortcut. I was super excited when I first saw that javascript was getting a function shortcut similar to the lambda syntax in my other favorite language, C#. However, it comes with what - is to me at least - a pretty unfortunate cost. Lexical scope.

Lexical is a fancy way of saying that the scope of a function is unchangeable. Most of the time lexical (or static) scope is something you won't even care about. It won't negatively affect the code I wrote in the sample above, but what about when it _does_ affect your code? Will it be obvious enough that `=>` is causing odd scope behaviors? The answer is: it will if you *remember* that the `=>` operator isn't just a function operator, it's a static scope operator.

Which brings me to the first thing I dislike about the "arrow functions" in es6: They aren't called "static scope" operators. I think we're doing a huge disservice to new and experienced javascript devs by not being upfront about what this operator really does. Saying "this is a nifty new shortcut to make all your functions" isn't the same as "this creates a function with a static scope".

I mentioned the second thing I don't like about the static scope operator (yes I'm calling it that from now on. come at me): scope errors aren't super obvious when they happen. Which makes things that "should just work" completely fail.

And the third and final thing that I don't like about the static scope operator is that it often leads to code hoop-jumping where you still end up with - again, to me - less readable code.

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
