# Contributing to `node-rdkafka`

:+1::tada: First off, thanks for taking the time to contribute! :tada::+1:

The following is a set of guidelines for contributing to `node-rdkafka`
which is hosted in the [Blizzard Organization](https://github.com/blizzard)
on GitHub. This document lists rules, guidelines, and help getting started,
so if you feel something is missing feel free to send a pull request.

#### Table Of Contents

[What should I know before I get started?](#what-should-i-know-before-i-get-started)
  * [Contributor Agreement](#contributor-agreement)

[How Can I Contribute?](#how-can-i-contribute)
  * [Reporting Bugs](#reporting-bugs)
  * [Suggesting Enhancements](#suggesting-enhancements)
  * [Pull Requests](#pull-requests)

[Styleguides](#styleguides)
  * [Git Commit Messages](#git-commit-messages)
  * [JavaScript Styleguide](#javascript-styleguide)
  * [C++ Styleguide](#c++-styleguide)
  * [Specs Styleguide](#specs-styleguide)
  * [Documentation Styleguide](#documentation-styleguide)

[Debugging](#debugging)
  * [Debugging C++](#debugging-c)

[Updating librdkafka version](#updating-librdkafka-version)

## What should I know before I get started?

### Contributor Agreement

Not currently required.

## How can I contribute?

### Reporting Bugs

Please use __Github Issues__ to report bugs. When filling out an issue report,
make sure to copy any related code and stack traces so we can properly debug.
We need to be able to reproduce a failing test to be able to fix your issue
most of the time, so a custom written failing test is very helpful.

Please also note the Kafka broker version that you are using and how many
replicas, partitions, and brokers you are connecting to, because some issues
might be related to Kafka. A list of `librdkafka` configuration key-value pairs
also helps.

### Suggesting Enhancements

Please use __Github Issues__ to suggest enhancements. We are happy to consider
any extra functionality or features to the library, as long as they add real
and related value to users. Describing your use case and why such an addition
helps the user base can help guide the decision to implement it into the
library's core.

### Pull Requests

* Include new test cases (either end-to-end or unit tests) with your change.
* Follow our style guides.
* Make sure all tests are still passing and the `linter` does not report any issues.
* End files with a new line.
* Document the new code in the comments (if it is JavaScript) so the
  documentation generator can update the reference documentation.
* Avoid platform-dependent code.
  <br>**Note:** If making modifications to the underlying C++, please use built-in
  precompiler directives to detect such platform specificities. Use `Nan`
  whenever possible to abstract node/v8 version incompatibility.
* Make sure your branch is up to date and rebased.
* Squash extraneous commits unless their history truly adds value to the library.

## Styleguides

### General style guidelines

Download the [EditorConfig](http://editorconfig.org) plugin for your preferred
text editor to automate the application of the following guidelines:

* Use 2-space indent (no tabs).
* Do not leave trailing whitespace on lines.
* Files should end with a final newline.

Also, adhere to the following not enforced by EditorConfig:

* Limit lines to 80 characters in length. A few extra (<= 5) is fine if it helps
  readability, use good judgement.
* Use `lf` line endings. (git's `core.autocrlf` setting can help)

### Git Commit Messages

Commit messages should adhere to the guidelines in tpope's
[A Note About Git Commit Messages](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)

In short:

* Use the imperative mood. ("Fix bug", not "Fixed bug" or "Fixes bug")
* Limit the first line to 50 characters or less, followed by a blank line
  and detail paragraphs (limit detail lines to about 72 characters).
* Reference issue numbers or pull requests whenever possible.

### JavaScript Styleguide

* Place `module.exports` at or near the top of the file.
  * Defined functions are hoisted, so it is appropriate to define the
    function after you export it.
  * When exporting an object, define it first, then export it, and then add
    methods or properties.
* Do not use ES2015 specific features (for example, do not use `let`, `const`,
  or `class`).
* All callbacks should follow the standard Node.js callback signature.
* Your JavaScript should properly pass the linter (`make jslint`).

### C++ Styleguide

* Class member variables should be prefixed with `m_`.
* Use a comment when pointer ownership has changed hands.
* Your C++ should properly pass the `cpplint.py` in the `make lint` test.

### Specs Styleguide

* Write all JavaScript tests by using the `mocha` testing framework.
* All `mocha` tests should use exports syntax.
* All `mocha` test files should be suffixed with `.spec.js` instead of `.js`.
* Unit tests should mirror the JavaScript files they test (for example,
  `lib/client.js` is tested in `test/client.spec.js`).
* Unit tests should have no outside service dependencies. Any time a dependency,
  like Kafka, exists, you should create an end-to-end test.
* You may mock a connection in a unit test if it is reliably similar to its real
  variant.

### Documentation Styleguide

* Write all JavaScript documentation in jsdoc-compatible inline comments.
* Each docblock should have references to return types and parameters. If an
  object is a parameter, you should also document any required subproperties.
* Use `@see` to reference similar pieces of code.
* Use comments to document your code when its intent may be difficult to understand.
* All documentation outside of the code should be in Github-compatible markdown.
* Make good use of font variations like __bold__ and *italics*.
* Use headers and tables of contents when they make sense.

## Editor

I began using Visual Studio code to develop on `node-rdkafka`. If you use it you can configure the C++ plugin to resolve the paths needed to inform your intellisense. This is the config file I am using on a mac to resolve the required paths:

`c_cpp_properties.json`
```
{
    "configurations": [
        {
            "name": "Mac",
            "includePath": [
                "${workspaceFolder}/**",
                "${workspaceFolder}",
                "${workspaceFolder}/src",
                "${workspaceFolder}/node_modules/nan",
                "${workspaceFolder}/deps/librdkafka/src",
                "${workspaceFolder}/deps/librdkafka/src-cpp",
                "/usr/local/include/node",
                "/usr/local/include/node/uv"
            ],
            "defines": [],
            "macFrameworkPath": [
                "/Library/Developer/CommandLineTools/SDKs/MacOSX10.14.sdk/System/Library/Frameworks"
            ],
            "compilerPath": "/usr/bin/clang",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "clang-x64"
        }
    ],
    "version": 4
}
```

## Debugging

### Debugging C++

Use `gdb` for debugging (as shown in the following example).

```
node-gyp rebuild --debug

gdb node
(gdb) set args "path/to/file.js"
(gdb) run
[output here]
```

You can add breakpoints and so on after that.

## Updating librdkafka version

The librdkafka should be periodically updated to the latest release in https://github.com/edenhill/librdkafka/releases

Steps to update:
1. Update the `librdkafka` property in [`package.json`](https://github.com/Blizzard/node-rdkafka/blob/master/package.json) to the desired version.

1. Update the librdkafka git submodule to that versions release commit (example below)

    ```bash
    cd deps/librdkafka
    git checkout 063a9ae7a65cebdf1cc128da9815c05f91a2a996 # for version 1.8.2
    ```

    If you get an error during that checkout command, double check that the submodule was initialized / cloned! You may need to run `git submodule update --init --recursive`

1. Update [`config.d.ts`](https://github.com/Blizzard/node-rdkafka/blob/master/config.d.ts) and [`errors.d.ts`](https://github.com/Blizzard/node-rdkafka/blob/master/errors.d.ts) TypeScript definitions by running:
    ```bash
    node ci/librdkafka-defs-generator.js
    ```
    Note: This is ran automatically during CI flows but it's good to run it during the version upgrade pull request.

1. Run `npm install` to build with the new version and fix any build errors that occur.

1. Run unit tests: `npm run test`

1. Run end to end tests: `npm run test:e2e`. This requires running kafka & zookeeper locally.

1. Update the version numbers referenced in the [`README.md`](https://github.com/Blizzard/node-rdkafka/blob/master/README.md) file to the new version.

## Publishing new npm version

1. Increment the `version` in `package.json` and merge that change in.

1. Create a new github release. Set the tag & release title to the same string as `version` in `package.json`.
