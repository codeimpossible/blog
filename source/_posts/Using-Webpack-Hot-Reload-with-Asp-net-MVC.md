---
title: Using Webpack Hot Reload with Asp.net MVC
date: 2016-01-31 22:36:28
tags:
---

I started a side project a while back to learn [React](), [Webpack]() and play around with [Simple.Data]() some more. Seriously, if you haven't had the joy of using simple.data as your data layer, it's tops. It's the bees knees.

One of the features of webpack that I was most interested in was the [hot-reloading of modules](https://github.com/webpack/docs/wiki/hot-module-replacement-with-webpack) in your bundle (or pack). There are a lot of other tools that do something similar, hell even visual studio has a extension called Web Essentials that adds live reload as a feature.

## Live Reload != Hot Reload

Those two things might sound really close but they are different in a few key places. Both watch your source files and do whatever transpilation/gulp is necessary to get your files ready to be served in your app. But live reload will trigger a reload of your webpage so the new javascript/css gets downloaded and executed. Hot Reload does this without causing a page refresh. Like none.

Webpack adds in a small runtime whenever it serves your hot reload enabled code. This runtime listens for signals from the webpack developer server (more on this in a minute) that say which module(s) were just changed. Once webpack knows which modules have changed it can look at the graph of dependencies in your application code (because it had to generate that to assemble your pack in the first place) and replace/re-execute those modules. This is pretty awesome, because it means when you change your `jsx` file so that it includes a totally awesome picture of some rando blogger from the internet:

```javascript
import React from 'react';

export default class CodeimpossibleIsAwesome extends React.Component {
  render() {
    return (
      <div className="codeimpossible-rules">
        <img src="http://www.gravatar.com/avatar/55997cec217233703cbf68b689578771?s=256" height="256" width="256" style="margin: 0 auto; display:block;"/>
        <h2 style="text-align: center">JAVSCRIPT 4 LYFE</h2>
      </div>
    );
  }
}
```

Webpack will pick that up, re-pack everything and reload the react class in your browser so the new view gets rendered. All without a new web request!

## Adding to Asp.net MVC

First, let's make sure you've got everything installed. Install [nodejs] if you don't have it already. Then install `gulp` and `webpack`

```bash
$ npm install -g gulp webpack
```

 - need to figure out why i'm [using unshift to push two urls to the front of the entry points part of the webpack config...](https://github.com/codeimpossible/proggr/blob/master/Proggr.All/WebApp/UX/gulpfile.js#L59)
