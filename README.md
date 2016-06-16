# edX ESLint config

ESLint config for edX JavaScript code.

## Table of Contents

1. [Overview](#overview)
2. [License](#license)
3. [Dependencies](#dependencies)
4. [Usage](#usage)
5. [The Config](#the-config)

## Overview

In order to standardize and enforce edX's JavaScript coding style across multiple codebases, edX has adopted ESLint and this shared ESLint config. This document describes the edX JavaScript style and its inspirations.

## License

The code in this repository is licensed the Apache 2.0 license unless otherwise
noted. Please see the [LICENSE file](https://github.com/edx/eslint-config-edx/blob/master/LICENSE) for details.

## Dependencies

[ESLint](http://eslint.org) is required to use `eslint-config-edx`, as it is an [ESLint shareable config](http://eslint.org/docs/developer-guide/shareable-configs).

## Usage

To begin using the edX ESLint config in a codebase, first install ESLint and the config:

    npm install --save-dev eslint eslint-config-edx

Then, configure your project's ESLint config to extend `eslint-config-edx` (see the ESLint docs on [Using a Shareable Config](http://eslint.org/docs/developer-guide/shareable-configs#using-a-shareable-config)). If you do not plan on adding custom rules beyond those in `eslint-config-edx` to your project, the easiest place to configure this is probably in your `package.json`:

    "devDependencies": {
        "eslint-config-edx": "^1.0.0"
    },
    "eslintConfig": {
        "extends": "eslint-config-edx"
    }

If you plan on adding additional customization, you can [configure your project with an `.eslintrc.js`/`.eslintrc.json` file](http://eslint.org/docs/user-guide/configuring#configuration-file-formats).

## The Config

Let's talk about what's in this ESLint config. edX maintains high-level documentation on the [Open edX Developer's Guide](http://edx.readthedocs.io/projects/edx-developer-guide/en/latest/user_interface_development.html?highlight=ux#adding-a-ui-page) on how we write new JavaScript features, including our use of RequireJS, Underscore and Backbone. This document seeks to be a lower-level guide on the semantics of how we write JavaScript, including nuts and bolts about spacing, syntax, and variable naming.

For the most part, edX follows the thoroughly documented [Airbnb JavaScript Style Guide for ES5](https://github.com/airbnb/javascript/tree/master/es5). Airbnb's JavaScript style is becoming the community standard, and their `eslint-config-airbnb` is the most-downloaded ESLint config on NPM. Because the main Airbnb ESLint config assumes use of ES6, and edX's JavaScript is primarily written in ES5, this config is an extension of `eslint-config-airbnb-base/legacy`, Airbnb's styleguide for ES5.

In addition to the base Airbnb rules, edX adds or extends several of our own. They are described below.

####[`camelcase`](http://eslint.org/docs/rules/camelcase)
- **Setting**: `["error", {"properties": "always"}]`
- **Explanation**: Use camelCase for JavaScript variable, function and property names. (Airbnb style is to not enforce property names.)
- **Example**:

        var useVariableNamesLikeThis,
            andLikeThis = {
                evenHere: function yepHereToo() {

                }
            };


####[`func-names`](http://eslint.org/docs/rules/func-names)
- **Setting**: `"off"`
- **Explanation**: Ignore this rule, it doesn't play nicely with RequireJS code.

####[`indent`](http://eslint.org/docs/rules/indent)
- **Setting**: `["error", 4]`
- **Explanation**: edX is standardized on indenting all code with four spaces. The JavaScript community generally prefers two spaces; edX uses four because of our use of Python (see [PEP8](https://www.python.org/dev/peps/pep-0008/)) and the desire to have consistency in our codebases.
- **Example**:

        // Correct pattern
        var example = function () {
            if (numberOfSpaces !== 4) {
                throw new Error('Use four spaces for indentation.');
            }
        };

####[`max-len`](http://eslint.org/docs/rules/max-len)
- **Setting**: `["error", 120]`
- **Explanation**: Use a maximum of 120 characters on a line. (The Airbnb default is 100, which we feel is too low.)

####[`new-cap`](http://eslint.org/docs/rules/new-cap)
- **Setting**: `"off"`
- **Explanation**: Disable the rule stating functions can only start with a capital letter if they are constructors.
- **Example**:

        // Correct pattern
        var htmlFoo = HtmlUtils.HTML('foo');

####[`no-else-return`](http://eslint.org/docs/rules/no-else-return)
- **Setting**: `"off"`
- **Explanation**: An `else` block after an `if` which contains a `return` is allowed. Technically the `else` is redundant (and Airbnb disallows it), but this rule allows for more readable control flow.
- **Example**:

        // Correct pattern
        if (fooString === 'foo') {
            return 30;
        } else {
            return 50;
        }

####[`no-invalid-this`](http://eslint.org/docs/rules/no-invalid-this)
- **Setting**: `"error"`
- **Explanation**: Do not use `this` outside of classes. Using strict mode, `this` can be undefined in these cases.
- **Example**: See [the ESLint `no-invalid-this` docs](http://eslint.org/docs/rules/no-invalid-this) for examples of allowed and disallowed behavior.

####[`no-shadow`](http://eslint.org/docs/rules/no-shadow)
- **Setting**: `"error"`
- **Explanation**: Do not redefine variables in a child scope that are already declared in the upper scope.
- **Example**:

        // Linter error
        var foo = 'bar';
        if (fizz) {
            var foo = 'buzz';
        }

####[`object-curly-spacing`](http://eslint.org/docs/rules/object-curly-spacing)
- **Setting**: `["error", "never"]`
- **Explanation**: Do not insert extra spaces inside of curly brackets.
- **Example**:

        // Correct pattern
        var obj = {foo: 42}

        // Linter error
        var obj2 = { foo: 42 }

####[`one-var`](http://eslint.org/docs/rules/one-var)
- **Setting**: `["error", "always"]`
- **Explanation**: Only use one variable declaration per function. This reinforces awareness of ES5's block scoping and hoisting, which can reduce bugs.
- **Example**:

    // Correct pattern
    function () {
        var foo, bar, baz,
            fizz = 'buzz',
            foz = 49;
    }
    // Linter error
    function () {
        var fizz = 'buzz';

        if (foo) {
            var baz = 'bar';
        }
    }

####[`one-var-declaration-per-line`](http://eslint.org/docs/rules/one-var-declaration-per-line)
- **Setting**: `["error", "initializations"]`
- **Explanation**: Variables that are not initialized can be declared on the same line. Only one initialization is allowed per line.
- **Example**:

        // Correct pattern
        var foo, bar,
            fizz = 'buzz';

        // Linter error
        var foo, bar, fizz = 'buzz';

####[`strict`](http://eslint.org/docs/rules/strict)
- **Setting**: `["error", "function"]`
- **Explanation**: Every top-level function declaration must have a `"use strict";` in it. Do not use `"use strict";` anywhere else.
- **Example**: See the [ESLint `strict` rule docs](http://eslint.org/docs/rules/strict) and the [MDN strict mode docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode).
