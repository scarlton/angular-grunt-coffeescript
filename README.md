# angular-grunt-coffeescript - Seed project for large CoffeeScript AngularJS projects

## Motivation

* I needed a seed project so that I could get a real [AngularJS](http://angularjs.org/) application up and running quickly, with all the fixin's
* I wanted to share some conventions that have worked well for a large CoffeeScript AngularJS project

### But I code in Javascript!

All of the conventions and lessons-learned demonstratred here can be applied to a Javascript-based project.  

## Features

* Pure [CoffeeScript](http://coffeescript.org/)
* [Grunt](http://gruntjs.com)-based build script that:
    * Starts a server and immediately deploys changes as you code
    * Allows for a clean seperation between the organization of your source code and that of your served files
    * A minified and concatenated distribution
* Incorporates most major aspects of AngularJS
    * Modules, directives, views, services, controllers, filters, dependency injection, etc.
* Demonstrates 3rd party library integration ([jQueryUI](http://jqueryui.com/) and a 'global' library wrapped in a module, in this case [toastr](http://codeseven.github.com/toastr/)).
* Opinionated conventions for:
    * naming Modules and Providers
    * sharing services, directives, filters across views

## Conventions

All of these are demonstrated in the example code.

### Directories & Files

    src/             
      main/              # application root
        index.html       # good ol' index.html
        app/             # contains all application files
          app.coffee     # handles routing, module wiring, and bootstrapping
          ...            # a directory per 'package'.  Described later
        lib/             # all 3rd party code
          ...            # a directory per 3rd party lib
      test/              # all test files and code
        TODO             # TODO incorporate Testacular into build

#### Packages

A 'package' is a loose term I'm using for a grouping of related artifacts.  Best explained by example:

* All files for the app's 'details view' will go into the detailsView directory.  None of the files in this directory will be used outside of this view
* 'Common' code that's shared between views are grouped under the 'common' directory

The structure you use for these groupings is up to you, but I've found that the 'xxxView' and 'common' structure has worked very well.

### Modules and 'Providers'

 Because we're using dependency injection, we don't need to use any javascript namespacing.  Everything in CoffeeScript is in an anonymous function and so never global, and everything we need we make sure is available in a module.

The key, then, becomes the naming convention we use for both our modules and the 'providers' (eg values, factories, directives, filters).

 - We use one module per Provider.  We don't group providers together (I don't see the benefit).
 - Module name matches the name and path of the file in which it's defined.  So the module for the app's common Data service would be `common.services.dataSvc`
 - Provider name depends on the type of provider.
     - For *values* and *factories*, which are generally used to provide Controllers, Services, and wrappers around 3rd parties libs, the *name of my provider is the exact same name as the module*.
     - For *directives* and *filters*, we do not namespace.  Because they have to be referenced in the DOM, short names are best for directives and filters.  See the code for examples.

 Having one module per provider makes wiring easy - We just list all of our modules in `app.coffee`.

### Misc Conventions

* Never use a global function for a controller, always a module
* Always use the 'array' format for dependency injection

## Building and Running

### Initial setup

1. Ensure that you have [node](http://nodejs.org/), [npm](https://npmjs.org/), and [grunt](http://gruntjs.com/) installed and in your path
1. Clone this repository
1. Run `npm install` to pull down all build-time dependencies

### Start server and watch for changes

This is the default grunt task.  To run the default grunt task, run `grunt --config grunt.coffee`.  Point the browser to `http://localhost:8000`

### Distribution Build

A distribution build minifies your js and css.  Run `grunt --config grunt.coffee dist`

## Build Output

When you run the default Grunt task, you will see this output in your project's directory.  `target/main` is what is served to the browser.

    target/             
      main/              # all application files and code
        index.html       # your old friend index.html
        js/
          app.js         # all of your application's code, concatenated
          lib.js         # all 3rd party library code, concatenated
        style/
          app.css        # all of your application's css, concatenated
          lib.css        # all 3rd party library css, concatenated
        ...              # a directory per 'package', with all the static files (html, imgs, etc)
      test/              # all test files and code
        TODO             # TODO incorporate Testacular into build.  Doesn't exist yet!

## TODOs

 - End-to-end test and further test coverage
 - Move the hardcoded data to `$httpBackend`
 - Create a grunt init template

## Thanks

Some of the ideas here are my own but many are built upon the work of others:

* The folks that contributed to [this thread](https://groups.google.com/forum/#!topic/angular/O_3mlKiW-OQ/discussion)
* James Wanga and his [angular-sprout](https://github.com/thedigitalself/angular-sprout) project
* The [AngularJS Team](https://github.com/angular/angular.js/graphs/contributors)