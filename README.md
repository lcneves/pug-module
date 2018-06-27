# pug-module

Compiles multiple .pug files into a module that can be used in Node, Browserify or Webpack apps with `require`.

## Getting started

```
$ npm install pug-module --save-dev
$ npm install pug-runtime --save
```

package.json:
```
"scripts": {
  "build:pug": "pug-module views/*.pug -o lib/templates.js",
  // [...]
```

Back to the console:
```
npm run build:pug
```

Your app:
```
const templates = require('lib/templates.js');

console.log(templates.helloWorld());
// <div><p>Hello world!</p></div>

console.log(templates.helloParameter({ parameter: 'Pug' });
// <div><p>Hello Pug!</p></div>
```

## Usage

```
node_modules/.bin/pug-module [options] path/to/files*.pug -o path/to/compiled.js
```

### Options

`-q, --quiet`: Do not output success message to console.

`-r, --recursive`: Find all pug files under the specified directories
and generate camelcase name from the relative directory/file path.
Given mydir as a parameter and mydir/foo/bar/helloWorld.pug, the module name will resolve to fooBarHelloWorld(...).

A directory structure like this:
```
mail
--welcome
----subject.pug
----html.pug
--confirm
----subject.pug
----html.pug
```
will generate the modules
`welcomeSubject`, `welcomeHtml`, `confirmSubject`, and `confirmHtml`.

### How is the template function name composed?

The `pug-module` tool will read each `.pug` filename, strip the extension, convert it to camelCase and strip all non-alphanumeric characters.

```
views/some_cool-template+2.pug
// compiles to
module.exports['someCoolTemplate2'] = function (...)
```
