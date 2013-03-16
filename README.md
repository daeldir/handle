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

