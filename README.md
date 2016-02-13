# [![CSJS logo](https://cdn.rawgit.com/rtsao/csjs/logo/logo.svg "CSJS (Cascading Style JavaScripts)")](https://github.com/rtsao/csjs)

[![Join the chat at https://gitter.im/rtsao/csjs](https://badges.gitter.im/rtsao/csjs.svg)](https://gitter.im/rtsao/csjs?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![build status][build-badge]][build-href]
[![coverage status][coverage-badge]][coverage-href]
[![dependencies status][deps-badge]][deps-href]

> CSJS allows you to write modular, scoped CSS with valid JavaScript.

## Features
* Extremely simple and lightweight
  * Zero dependencies, [~2KB minified and gzipped][bundle]
* Leverages native ES6 and CSS features <sup>[(1)]</sup> rather than reinventing the wheel
  * Seamless scoped styles and dead-simple variables/mixins using tagged ES6 template strings
  * Modular styles with explicit dependencies powered by native CommonJS/ES6 modules
  * Style composition via natural class composition mechanics already in CSS/HTML
* Works tooling-free; no required transpilation/compilation/build steps <sup>[(2)]</sup>
* Framework-agnostic (No React dependency; works with Web Components, etc.)
* Fully supported native CSS media queries, pseudo-classes, keyframe animations, etc.

## Quick Example

```javascript
const csjs = require('csjs');
const ReactDOM = require('react-dom/server');
const {div, h1} = require('react').DOM;

const green = '#33aa22';

const styles = csjs`

  .panel {
    border: 1px solid black;
    background-color: ${green};
  }
  
  .title {
    padding: 4px;
    font-size: 15px;
  }

`;

const html = ReactDOM.renderToStaticMarkup(
  div({className: styles.panel}, [ 
    h1({className: styles.title}, 'Hello World!')
  ])
);
/*
<div class="panel_4EC9fB">
  <h1 class="title_4EC9fB">Hello World!</h1>
</div>
*/

const css = require('csjs/get-css')(styles);
/*
.panel_4EC9fB {
  border: 1px solid black;
  background-color: #33aa22;
}

.title_4EC9fB {
  padding: 4px;
  font-size: 15px;
}
*/
```

### Simple, tooling-free

CSJS runs in ES6 environments without transpilation, compilation, or build steps (including Node 4+ and latest stable Chrome/Firefox/Safari/Edge).

[![sauce labs test status][sauce-badge]][sauce-href]

Of course, you can always use Babel to transpile ES6 template strings and polyfill ES6 symbols, allowing you to use CSJS in any ES5 environment.

### Framework-agnostic

CSJS works with any framework, be it React, native Web Components, or something else.

### Full power of JavaScript in your CSS

* Real, full-fledged JavaScript
* Obviates the need for Sass/Less or other preprocessors
 * Proper imports/require
 * Real variables, functions, loops, etc.
 * As extensible as JavaScript itself

### Class Composition CSS Syntax

CSJS also includes an (optional) handy class composition syntax:

**common-styles.js**
```javascript
const csjs = require('csjs');

module.exports = csjs`

  .foo {
    border: 1px solid black;
  }
  
  .bar {
    padding: 4px;
    font-family: serif;
    font-size: 15px;
  }

`;

```

**button-styles.js**
```javascript
const csjs = require('csjs');

const common = require('./common-styles');

module.exports = csjs`

  .button extends ${common.foo} {
    background: #ccc;
    border-radius: 4px;
  }
  
  .superButton extends .button, ${common.bar} {
    background: red;
    font-weight: bold;
  }

`;

```

**component.js**
```javascript

var buttonStyles = require('./button-styles');
var getCss = require('csjs/get-css');

buttonStyles.superButton;
// => "foo_19Yy9 bar_19Yy9 button_4fdaQ superButton_4fdaQ"

getCss(buttonStyles);
/*
  .button_4fdaQ {
    background: #ccc;
    border-radius: 4px;
  }
  
  .superButton_4fdaQ {
    background: red;
    font-weight: bold;
  }

*/
```

### Optional tooling

#### Extracted static CSS bundles

[csjs-extractify](https://github.com/rtsao/csjs-extractify) is a browserify plugin that will allow you to extract your application's CSJS into a static CSS file at build time.

#### Automatic CSS injection

[csjs-injectify](https://github.com/rtsao/csjs-injectify) is a browserify transform that will automatically inject your scoped CSS into the `<head>`. It is recommended to use this rather than the [csjs-inject](https://github.com/rtsao/csjs-inject) module directly.


## FAQ

##### Why the name CSJS?

CSJS is 100% valid JavaScript, hence the name Cascading Style JavaScripts.

##### Why not Sass?

Sass doesn't provide any way to scope CSS, thus encapsulation of styles in components isn't possible with Sass alone. Additionally, because Sass was designed for use in a global CSS namespace, many of its features just don't make sense when styles are scoped and encapsulated in components. `@extend` in Sass is extremely problematic, whereas CSJS has a proper mechanism for class composition that actually works like it should. Furthermore, with CSJS, you have the ability to use real JavaScript in CSS, which is significantly more powerful and extensible than the language features included in Sass, so there's not really any reason to use Sass at all.

##### Why not CSS Modules?

CSJS was inspired by [CSS Modules] and they are virtually identical in concept. However, the key difference is CSS Modules attempts to reproduce an ES6-style module system into your CSS, whereas CSJS simply uses native JS modules and provides a convenient way to write scoped, modular CSS.

One advantage is CSJS is valid JavaScript so it works without any extra tooling (CSS Modules is not valid CSS). Also, because CSJS is essentially an amalgamation of plain JavaScript and plain CSS, you don't have to learn any new syntax or semantics.

##### Why not Radium?

Inline styles are cool, but there are limitations to using pure inline styles. For example, CSS pseudo-classes and media queries aren't possible with inline styles. This is the premise behind Radium, which works around this by re-implementing these CSS features using JavaScript.

Whereas Radium is wholly dependent on React and involves performance tradeoffs in its JavaScript implementations of CSS features, CSJS works regardless of framework (or lack thereof) and allows for the use of all CSS features natively (including media queries and pseudo-classes).

## See Also
* https://github.com/rtsao/csjs-example-app
* https://github.com/rtsao/csjs-extractify
* https://github.com/rtsao/csjs-injectify
* https://github.com/rtsao/csjs-inject
* https://github.com/rtsao/scope-styles

## License
MIT

[(1)]: #full-power-of-javascript-in-your-css
[(2)]: #simple-tooling-free
[CSS Modules]: https://github.com/css-modules/css-modules
[bundle]: https://www.brcdn.org/csjs/latest?standalone=csjs&uglify=true

[build-badge]: https://travis-ci.org/rtsao/csjs.svg?branch=master
[build-href]: https://travis-ci.org/rtsao/csjs
[coverage-badge]: https://coveralls.io/repos/rtsao/csjs/badge.svg?branch=master&service=github
[coverage-href]: https://coveralls.io/github/rtsao/csjs?branch=master
[deps-badge]: https://david-dm.org/rtsao/csjs.svg
[deps-href]: https://david-dm.org/rtsao/csjs
[sauce-badge]: https://saucelabs.com/browser-matrix/csjs.svg
[sauce-href]: https://saucelabs.com/u/csjs
