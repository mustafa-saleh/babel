# Babel

## Introduction

Babel is a javascript compiler, it's a toolchain that is mainly used to convert ECMAScript 2015+ code into a backword compatible version of javascript in current and older browsers or environments.

```javascript
// Babel Input: ES2015 arrow function
[1, 2, 3].map((n) => n + 1);

// Babel Output: ES5 equivalent
[1, 2, 3].map(function (n) {
  return n + 1;
});
```

Babel has support for latest version of javascript through syntax transformers. These plugins allow you to use new syntax without waiting for browser support. If you'are using Typescript, babel can strip out type annotations with `@babel/preset-typescript`.  
Babel is built out of blugins, compose your own transformation pipeline using existing plugins or write your own. It has source map support so you can easily debug your compiled code.

## Usage Guide

To compile ES2015+ javascript syntax into code that works in current browsers, babel needs to transform the new syntax and add polyfills to the missing features. To get started with babel, install the required packages

```javascript
npm install --save-dev @babel/core @babel/cli
```

Babel transformations come in the form of plugins, which are small JavaScript programs that instruct Babel on how to carry out transformations to the code. You can even write your own plugins to apply any transformations you want to your code. To transform ES2015+ syntax into ES5 we can rely on official plugins like "@babel/plugin-transform-arrow-functions":

```javascript
npm i --save-dev @babel/plugin-transform-arrow-functions
npx babel src --out-dir lib --plugins=@babel/plugin-transform-arrow-functions
```

Now any arrow functions in our code will be transformed into ES5 compatible function expressions:

```javascript
const fn = () => 1;
// converted to
var fn = function fn() {
  return 1;
};
```

But instead of adding all plugins to transform all code features in your code, you can use presets which are predefined set of plugins. Just like plugins you can create your won presets to share any combination of plugins.

```javascript
npm install --save-dev @babel/preset-env
npx babel src --out-dir lib --presets=@babel/env
```

Rather than passing both cli and preset options from the terminal, let's look at another way of passing options through configuration files. Create config file named "babel.config.json" in the root of your project with the following content

```json
{
  "presets": [
    [
      "@babel/env",
      {
        "targets": {
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1"
        },
        "useBuiltIns": "usage",
        "corejs": "3.6.5"
      }
    ]
  ]
}
```

The config file defines an array of presets to activate when processing this file.  
"@babel/preset-env" is a smart preset that allows you to use the latest JavaScript without needing to micromanage which syntax transforms (and optionally, browser polyfills) are needed by your target environment(s).  
The "target" describes the environments you support/target for your project, this can either be a browserlist-compatible query or an object of minimum environment versions to support like the ones used above. By default, the preset will use [browserlist config sources](https://github.com/browserslist/browserslist#queries) unless either the "targets" or "ignoreBrowserListConfig" options are set.  
The "useBuiltIns" option configures how the preset handles polyfills. When either the usage or entry options are used, @babel/preset-env will add direct references to core-js modules as bare imports (or requires). This means core-js will be resolved relative to the file itself and needs to be accessible.  
The "corejs" option ensures that the preset injects the polyfills supported by your core-js version

To compile your code from "src" directory to "lib", execute the following command

```javascript
npx babel src --out-dir lib
```

```javascript

```
