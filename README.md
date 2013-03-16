handle
======

A CoffeeScript literate programming project manager

What is it?
-----------

`handle` is a utility whose goal is to help a special programming
practice in web development.

The requirements are the following:

 - We should be able to write our website in only one language:
   CoffeeScript. To this end, we can use CoffeeScript templates engines
   like
   [CoffeeTemplates](https://github.com/mikesmullin/coffee-templates)
   for HTML and
   [CoffeeStylesheets](https://github.com/mikesmullin/coffee-stylesheets)
   for CSS.
 - We should write our application with the new feature of literate
   programming of CoffeeScript.
 - We should be able to access the documentation of our code in HTML.
 - We should be able to write our code for the client the same way we
   write our code for the server (with CommonJS modules).
 - The compilation of HTML, javascript and CSS should be done once when
   starting the application, when possible.
 - Saving a file and reloading a page in the browser should work like
   the old "traditionnal" when development techniques, that is, the
   changes are visible without server restart.
 - The script should run the tests of our project at compilation time.

Usage
-----

**handle** _command_

 * TODO: handle watch: compile and start the server, and do it again at
   each file change.
 * TODO: handle run: compile and start the server.
 * TODO: handle safe-run: compile and start the server, and start it
   again if the server crash.
 * TODO: handle compile: compile the server.
 * TODO: handle bundle: compile the server and place all generated files
   in a directory, which can be started as a standalone application,
   without having to fetch the dependencies.
 * TODO: handle test: Only test the server, don't start it for browser
   access.
 * TODO: handle coverage: run tests and report code coverage of the
   tests.
 * TODO: handle completude: run feature tests and report the project
   advancement state.

How does it works?
------------------

The file you are currently reading is the implementation of `handle`,
written in literate CoffeeScript. Now that we explained what the program
should do, let's get into the code!

### Dependencies

We depend on many libraries. We will need `coffee-templates` to compile
our coffeescript templates into HTML, and `coffee-stylesheets` to
compile into CSS. We will also need `browserify` to generate a
javascript bundle loadable in the browser and coded with CommonJS module
system.

```coffeescript
    templates = require 'coffee-templates'
    stylesheets = require 'coffee-stylesheets'
    browserify = require 'browserify'
```

We also need to load the CoffeeScript library to compile the project
code into Javascript:

```coffeescript
    coffee = require 'coffee-script'
```

We will read files to compile the source code and walk the tree
directory. We also will be manipulating file paths.

```coffeescript
    fs = require 'fs'
    path = require 'path'
```

### Compilation

We know what code to compile thank to some conventions. In the project,
the file `server.coffee.md` is the entry point for the server side code,
and is the script ran when we issue `handle run`. `client.coffee.md` is
the entry point for the client side code. All remaining source code is
in the directory `src/`. In `views/` are the CSS and HTML templates. In
`public/` are some static ressources (images...). 

Almost all the compiled code – all the code loaded in the browser – goes
into a `cache/` directory, except the server side code that goes into
the `lib/` directory.

#### Compiling all the CoffeeScript

We begin by compiling all the CoffeeScripts files to Javascript. The
client-side bundle generation by browserify will come later, using the
Javascript version of our code.

First, we compile all the files from `./src` to `./lib`, creating
`./lib` if it does not exist.

```coffeescript
    srcToLib = ->
      return unless fs.existsSync 'src'
      fs.mkdirSync 'lib' unless fs.existsSync 'lib'
      walkTree 'src', (file) ->
        code = fs.readFileSync(file).toString()
        newPath = file
          .replace('src/', 'lib/')
          .replace('.coffee.md', '.js')
        js = coffee.compile code, literate: yes
        fs.writeFileSync newPath, js
```

The `walkTree` function recursively read all the files in a directory,
calling a function with those paths. The argument of the callback
function is the path of a file in that directory.

```coffeescript
    walkTree = (directory, callback) ->
      files = fs.readdirSync directory
      for file in files
        filePath = path.join directory, file
        if fs.statSync(filePath).isDirectory()
          walkTree filePath, callback
        else
          callback filePath
```


