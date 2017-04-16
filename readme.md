## EntryWrapper webpack plugin
[![Build Status](https://img.shields.io/travis/echopi/entry-wrap-webpack-plugin/entry-wrapper-webpack-plugin.svg)](https://travis-ci.org/echopi/entry-wrap-webpack-plugin)
[![node version](https://img.shields.io/badge/node.js-%3E=_4.3.0-green.svg)](http://nodejs.org/download/)

> Wrap your bundle entry points for webpack2. 

Since the `entry-wrap-webpack-plugin` author has no feedback to [my PR](https://github.com/shakyShane/entry-wrap-webpack-plugin/pull/5), I decide to publish my own package `entry-wrapper-webpack-plugin` for webpack2.

### Install 

```shell
npm i entry-wrapper-webpack-plugin -D
```

### Usage example

Example: You're adding a Webpack bundle to an existing project where jQuery
is being loaded externally by requireJS. You can tell Webpack to NOT bundle
your own version jQuery (as you want to re-use the external version), but then 
you need to wrap your entry point in a `require` call
to ensure jQuery is loaded before your bundle executes. 

That's what this plugin is for.

**`webpack.config.js`**

```shell
var EntryWrap = require('entry-wrapper-webpack-plugin');
var webpack = require('webpack');
module.exports = {
    context: __dirname,
    entry: "./input",
    output: {
        path: __dirname + "/dist",
        filename: "bundle.js"
    },
    externals: {
		"jquery": "jQuery"
	},
    plugins: [
        new EntryWrap('require(["jquery"], function (jQuery) {', '})',{
            test: /\.js$/
        })
    ]
};
```

**`input.js`**
```js
// jQuery will NOT be included in your bundle
const $ = require('jquery');
```

... Will result in something along the lines of

```js
require(["jquery"], function (jQuery) {
	// ... webpack bundle here
	// that will not run until jQuery is available on the page
})
```
