# atom-ternjs

> JavaScript code intelligence for atom with [Tern](https://github.com/ternjs/tern).
Uses suggestion provider by [autocomplete-plus](https://github.com/atom/autocomplete-plus).

## Installation

Configure your project
* Navigate to Packages -> Atom Ternjs -> Configure project
* The config view appears.
* Hit "Save & Restart Server" to create/update the .tern-project file

**Configure project doesn't support the plugins-section (yet).
Please add plugins manually by editing the .tern-project file as shown below.**

**In order to use third party plugins read the [Third party plugins](#third-party-plugins) section!**

**In order to use third party plugins from within your project's ```node_modules``` read the [Third party plugins local](#third-party-plugins-local) section! This is also an alternative if [Third party plugins](#third-party-plugins) aren't working.**

If configure project does not work for you
* In your project root create a file named .tern-project. See docs @ http://ternjs.net/doc/manual.html#configuration.
* Check path in loadEagerly
* Restart the server via *Packages -> Atom Ternjs -> Restart server*

Example `.tern-project` file (customize to your own needs):
```json
{
  "ecmaVersion": 6,
  "libs": [
    "browser",
    "jquery"
  ],
  "loadEagerly": [
    "absolute/or/relative/path/to/your/js/**/*.js"
  ],
  "dontLoad": [
    "absolute/or/relative/path/to/your/js/**/*.js"
  ],
  "plugins": {
    "complete_strings": {},
    "node": {},
    "lint": {},
    "angular": {},
    "requirejs": {},
    "modules": {},
    "es_modules": {},
    "doc_comment": {
      "fullDocs": true
    }
  }
}
```

## EcmaVersion
* 5: use ECMAScript5
* 6: use ECMAScript6 (default)

## Libs
* browser: completion for vanilla js (optional)
* jquery: completion for jQuery (optional)

## Options
* loadEagerly: provide the path to your projects js. Absolute path is recommended, but not necessary. (For relative path do not use `./` as a prefix. This sometimes leads to an unexpected behaviour)
* dontLoad: can be used to prevent Tern from loading certain files. It also takes an array of file names or glob patterns.

## Plugins
* complete_strings: When enabled, this plugin will gather (short) strings in your code, and completing when inside a string will try to complete to previously seen strings. Takes a single option, maxLength, which controls the maximum length of string values to gather, and defaults to 15. (optional)
* doc_comment: tern will look for JSDoc-style type declarations. Returns the full comment text instead of the first sentence. (optional)
* node: NodeJS (optional)
* lint: <a href="https://github.com/angelozerr/tern-lint">angelozerr/tern-lint</a> is a tern plugin which is able to validate JavaScript files to collect semantic errors. <a href="https://github.com/AtomLinter/Linter">Linter</a> is used to display these errors and warnings (optional)
* angular: AngularJS (optional)
* requirejs: RequireJS (optional, understand RequireJS-style dependency management)
* modules: This is a supporting plugin to act as a dependency for other module-loading and module-resolving plugins. (optional, required by es_modules)
* es_modules: Builds on top of the modules plugin to support ECMAScript 6’s import and export based module inclusion. (optional, requires modules)
* For a list of possible plugins goto http://ternjs.net/doc/manual.html#plugins

## Third party plugins
In order to use third party plugins (e.g. [tern-node-express](https://github.com/angelozerr/tern-node-express)):
```
$ cd ~/.atom/packages/atom-ternjs
$ npm install tern-node-express
```
Add the plugin to your .tern-project file:
```json
{
  "ecmaVersion": 6,
  "libs": [
    "browser"
  ],
  "loadEagerly": [
    "app/**/*.js"
  ],
  "plugins": {
    "node-express": {}
  }
}
```
Restart the server: *Packages -> Atom Ternjs -> Restart server*

If completion isn't working as expected, you may also have a look here: [Third party plugins local](#third-party-plugins-local) section!

## Third party plugins local
This is a bit hacky but necessary if a third party plugin isn't working. Any help to improve this is appreciated.
This example uses [tern-node-express](https://github.com/angelozerr/tern-node-express).

```
$ cd ~/[your-project]
$ npm install tern-node-express
```

* Be sure ```tern``` and ```tern-node-express``` aren't installed via ```npm install -g```
* You can check this by using ``` $ npm list -g --depth=0 ```
* navigate to ```[your-project]/node_modules/tern-node-express```
* open node-express.js

Replace:
```
(function(mod) {
  if (typeof exports == "object" && typeof module == "object") // CommonJS
    return mod(require("tern/lib/infer"), require("tern/lib/tern"));
  if (typeof define == "function" && define.amd) // AMD
    return define([ "tern/lib/infer", "tern/lib/tern" ], mod);
  mod(tern, tern);
})(function(infer, tern) { ... }});
```

With:
```
(function(mod) {
  return mod(process.__infer, process.__tern);
})(function(infer, tern) { ... }});
```

**If ```mod``` relies on other dependencies, just keep them as they are.**

Restart the server via *Packages -> Atom Ternjs -> Restart server*

## .tern-project created/modified
* After the file was created or has been modified, restart the server via *Packages -> Atom Ternjs -> Restart server*

## Features
* Completion (autocompletion triggers automatically, via <kbd>strg+space</kbd> or can be forced in any context with <kbd>ctrl+alt+space</kbd>)

![atom-ternjs](http://www.tobias-schubert.com/github/completion-1.png)

![atom-ternjs](http://www.tobias-schubert.com/github/completion-2.png)
* Find references (set your cursor position to one of variable, function or instance -> open context-menu and trigger "Find references" or use the shortcut <kbd>ctrl+shift+r</kbd> (mac), <kbd>ctrl+shift+e</kbd> (mac, linux)) Click any item in the generated reference-list and navigate directly to file and position

![atom-ternjs](http://www.tobias-schubert.com/github/reference-1.png)

* Documentation (displayed if a suggestion with a valid documentation is selected in the select-list)
* Find definition (set your cursor position to one of variable, function or instance -> open context-menu and trigger "Find definition", <kbd>alt+click</kbd> or use the shortcut <kbd>ctrl+alt+d</kbd>)
* Back from definition <kbd>ctrl+alt+z</kbd>
* Rename variable (set your cursor position to a variable -> open context-menu and trigger "Rename" or use the shortcut <kbd>ctrl+alt+c</kbd>)
