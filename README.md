# assemble-core [![NPM version](https://img.shields.io/npm/v/assemble-core.svg)](https://www.npmjs.com/package/assemble-core) [![Build Status](https://img.shields.io/travis/jonschlinkert/assemble-core.svg)](https://travis-ci.org/jonschlinkert/assemble-core)

> The core assemble application with no presets or defaults. All configuration is left to the implementor.

This library, assemble-core, was designed to give implementors and hackers the baseline features and API for creating rich and powerful node.js applications. Learn more about [what you can do with assemble-core](#about).

- [Install](#install)
- [Usage](#usage)
- [Examples](#examples)
- [API](#api)
  * [Templates API](#templates-api)
  * [File System API](#file-system-api)
    + [.src](#src)
    + [.dest](#dest)
    + [.copy](#copy)
    + [.symlink](#symlink)
  * [Task API](#task-api)
    + [.task](#task)
    + [.build](#build)
    + [.watch](#watch)
- [About](#about)
- [Related projects](#related-projects)
- [Tests](#tests)
  * [Running tests](#running-tests)
  * [Test coverage](#test-coverage)
- [Contributing](#contributing)
- [History](#history)
- [Author](#author)
- [License](#license)

_(TOC generated by [verb](https://github.com/verbose/verb))_

## Install

Install with [npm](https://www.npmjs.com/)

```sh
$ npm i assemble-core --save
```

## Usage

```js
var assemble = require('assemble-core');
var app = assemble();
```

## Examples

**view collections**

Create a custom view collection:

```js
var app = assemble();
app.create('pages');
```

Now you can add pages with `app.page()` or `app.pages()`:

```js
app.page('home.hbs', {content: 'this is the home page!'});
```

**render**

Render a view:

```js
var app = assemble();
app.create('pages');

app.page('foo', {content: 'Hi, my name is <%= name %>'})
  .use(function (view) {
    if (!view.contents) {
      view.contents = fs.readFileSync(view.path);
    }
  })
  .set('data.name', 'Brian')
  .render(function (err, res) {
    console.log(res.content);
    //=> 'Hi, my name is Brian'
  });
```

## API

### [Assemble](index.js#L22)

Create an `assemble` application. This is the main function exported by the assemble module.

**Params**

* `options` **{Object}**: Optionally pass default options to use.

**Example**

```js
var assemble = require('assemble');
var app = assemble();
```

### Templates API

Assemble has an extensive API for working with templates and template collections. In fact, the entire API from the [templates](https://github.com/jonschlinkert/templates) library is available on Assemble.

While we work on getting the assemble docs updated with these methods you can visit [the templates library](https://github.com/jonschlinkert/templates) to learn more about the full range of features and options.

***

### File System API

Assemble has the following methods for working with the file system:

* [src](#src)
* [dest](#dest)
* [copy](#copy)
* [symlink](#symlink)

Assemble v0.6.0 has full [vinyl-fs](http://github.com/wearefractal/vinyl-fs) support, so any [gulp](http://gulpjs.com) plugin should work with assemble.

#### .src

Use one or more glob patterns or filepaths to specify source files.

**Params**

* `glob` **{String|Array}**: Glob patterns or file paths to source files.
* `options` **{Object}**: Options or locals to merge into the context and/or pass to `src` plugins

**Example**

```js
app.src('src/*.hbs', {layout: 'default'});
```

#### .dest

Specify the destination to use for processed files.

**Params**

* `dest` **{String|Function}**: File path or custom renaming function.
* `options` **{Object}**: Options and locals to pass to `dest` plugins

**Example**

```js
app.dest('dist/');
```

#### .copy

Copy files from A to B, where `A` is any pattern that would be valid in [app.src](#src) and `B` is the destination directory.

**Params**

* `patterns` **{String|Array}**: One or more file paths or glob patterns for the source files to copy.
* `dest` **{String|Function}**: Desination directory.
* `returns` **{Stream}**: The stream is returned, so you can continue processing files if necessary.

**Example**

```js
app.copy('assets/**', 'dist/');
```

#### .symlink

Glob patterns or paths for symlinks.

**Params**

* `glob` **{String|Array}**

**Example**

```js
app.symlink('src/**');
```

***

### Task API

Assemble has the following methods for running tasks and controlling workflows:

* [task](#task)
* [build](#build)
* [watch](#watch)

#### .task

Define a task. Tasks are functions that are stored on a `tasks` object, allowing them to be called later by the [build](#build) method. (the [CLI](https://github.com/assemble/assemble-cli) calls [build](#build) to run tasks)

**Params**

* `name` **{String}**: Task name
* `fn` **{Function}**: function that is called when the task is run.

**Example**

```js
app.task('default', function() {
  return app.src('templates/*.hbs')
    .pipe(app.dest('dist/'));
});
```

#### .build

Run one or more tasks.

**Params**

* `tasks` **{Array|String}**: Task name or array of task names.
* `cb` **{Function}**: callback function that exposes `err`

**Example**

```js
app.build(['foo', 'bar'], function(err) {
  if (err) console.error('ERROR:', err);
});
```

#### .watch

Watch files, run one or more tasks when a watched file changes.

**Params**

* `glob` **{String|Array}**: Filepaths or glob patterns.
* `tasks` **{Array}**: Task(s) to watch.

**Example**

```js
app.task('watch', function() {
  app.watch('docs/*.md', ['docs']);
});
```

## About

**What can I do with it?**

You can use assemble-core to create your own custom:

* blog engine
* project generator / scaffolder
* e-book development framework
* build system
* landing page generator
* documentation generator
* front-end UI framework
* rapid prototyping application
* static site generator
* web application

## Related projects

Assemble is built on top of these great projects:

* [assemble](https://www.npmjs.com/package/assemble): Static site generator for Grunt.js, Yeoman and Node.js. Used by Zurb Foundation, Zurb Ink, H5BP/Effeckt,… [more](https://www.npmjs.com/package/assemble) | [homepage](http://assemble.io)
* [boilerplate](https://www.npmjs.com/package/boilerplate): Tools and conventions for authoring and publishing boilerplates that can be generated by any build… [more](https://www.npmjs.com/package/boilerplate) | [homepage](http://boilerplates.io)
* [composer](https://www.npmjs.com/package/composer): API-first task runner with three methods: task, run and watch. | [homepage](https://github.com/jonschlinkert/composer)
* [generate](https://www.npmjs.com/package/generate): Fast, composable, highly extendable project generator for node.js | [homepage](https://github.com/jonschlinkert/generate)
* [scaffold](https://www.npmjs.com/package/scaffold): Conventions and API for creating scaffolds that can by used by any build system or… [more](https://www.npmjs.com/package/scaffold) | [homepage](https://github.com/jonschlinkert/scaffold)
* [templates](https://www.npmjs.com/package/templates): System for creating and managing template collections, and rendering templates with any node.js template engine.… [more](https://www.npmjs.com/package/templates) | [homepage](https://github.com/jonschlinkert/templates)
* [verb](https://www.npmjs.com/package/verb): Documentation generator for GitHub projects. Verb is extremely powerful, easy to use, and is used… [more](https://www.npmjs.com/package/verb) | [homepage](https://github.com/verbose/verb)

## Tests

### Running tests

Install dev dependencies:

```sh
$ npm i -d && npm test
```

### Test coverage

As of December 16, 2015:

```
Statements : 100% (38/38)
Branches   : 100% (8/8)
Functions  : 100% (10/10)
Lines      : 100% (38/38)
```

## Contributing

Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](https://github.com/jonschlinkert/assemble-core/issues/new).

If Assemble doesn't do what you need, [please let us know][issue].

## History

**v0.7.0**

* Bumps [templates](https://github.com/jonschlinkert/templates) to 0.8.0 to take advantage of `isType` method for checking a collection type, and a number of improvements to how collections and views are instantiated and named.

**v0.6.0**

* Bumps [assemble-fs](https://github.com/jonschlinkert/assemble-fs) plugin to 0.5.0, which introduces `onStream` and `preWrite` middleware handlers.
* Bumps [templates](https://github.com/jonschlinkert/templates) to 0.7.0, which fixes how non-cached collections are initialized. This was done as a minor instead of a patch since - although it's a fix - it could theoretically break someone's setup

**v0.5.0**

* Bumps [templates](https://github.com/jonschlinkert/templates) to latest, 0.6.0, since it uses the latest [base-methods](https://github.com/jonschlinkert/base-methods), which introduces prototype mixins. No API changes.

**v0.4.0**

* Removed [emitter-only](https://github.com/doowb/emitter-only) since it was only includes to be used in the default listeners that were removed in an earlier release. In rare cases this might be a breaking change, but not likely.
* Adds lazy-cache
* Updates [assemble-streams](https://github.com/jonschlinkert/assemble-streams) plugin to latest

## Author

**Jon Schlinkert**

* [github/jonschlinkert](https://github.com/jonschlinkert)
* [twitter/jonschlinkert](http://twitter.com/jonschlinkert)

## License

Copyright © 2015 [Jon Schlinkert](https://github.com/jonschlinkert)
Released under the MIT license.

***

_This file was generated by [verb](https://github.com/verbose/verb) on December 16, 2015._