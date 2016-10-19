# Tiny `npm` Package Starter Kit

Helps quickly start coding tiny/small `npm` packages **using the latest ES syntax** and features while **publishing ES5 code** for maximum compatibility.

Standard settings are provided for:

- Convenience [`npm`](https://docs.npmjs.com/misc/scripts) scripts
- [ESLint](http://eslint.org/)
- [Babel](http://babeljs.io/)
- [Mocha](https://mochajs.org/)/[Chai](http://chaijs.com/)
- [istanbul/nyc](https://istanbul.js.org)

## Getting started

First of all, clone the repo:

```bash
$ git clone https://github.com/gabmontes/tiny-package-starter.git <my-package-name>
$ cd <my-package-name>
```

Then, do some cleanup and init the new package's repo:

```bash
$ rm -rf .git    # Cleans up git and unlinks from the upstream repo
$ git init       # Initializes git again
$ npm init       # Initializes npm
$ npm install    # Installs base packages
```

After the package's code is ready, just set the proper version and publish!

```bash
$ npm version <major|minor|patch>    # updates package.json and applies tag
$ npm publish                        # lints, builds and publishes
$ git push --follow-tags             # don't forget to push the code and tags!
```

### Watch out!

The `files` property in `package.json` specifies which files/folders will be published to the `npm` registry. As per initial settings, only the `dist`, `lib` and `bin` folders will be published to keep the installed package as clean and small as possible.

## Folder's structure

```
.
├── bin        # ES5 command line scripts
├── cli        # Source command line scripts
├── dist       # ES5 package distribution files
├── lib        # Source package files
└── test       # Tests folder
```

## Provided settings

### Convenience `npm` scripts

There are scripts to lint, test the code, run coverage tests, build and do it all together when executing `npm publish`!

### ESLint

Recommended configurations is provided for the package code. In addition, the `test` folder enables the `mocha` environment variables and, for convenience, the `bin` folder prevents errors/warnings for using `console.log()` statements.

Most customizations like adding configs, plugins and rules, should be done in the root's `.eslintrc.json` file so those are inherited to the whole package.

### Babel

Babel is preset with the [`latest-minimal`](https://github.com/gabmontes/babel-preset-latest-minimal) preset for development and testing, which loads only the minimum set of transforms based on the current execution environment (installed node's supported features).

For building, it uses the [`latest`](http://babeljs.io/docs/plugins/preset-latest/) preset instead, which enables all the standard transforms needed to convert the code to ES5 and ensuring maximum compatibility of the published `npm` package.

Experimental (stage-x) presets are not included by default.

#### Polyfills

While latest ES syntax features are transparently managed by the `latest`/`latest-minimal` presets, new global objects and object's functions shall be polyfilled.

Long story short, in code that will be distributed as a package, prefer using [`babel-plugin-transform-runtime`](http://babeljs.io/docs/plugins/transform-runtime/) ([why?](http://babeljs.io/docs/plugins/transform-runtime/)) unless the codes uses some of the new object's instance functions as `[1,2].includes(1)`. In that case, polyfill just what is needed through [`core-js`](https://github.com/zloirock/core-js) or polyfill all the things using [`babel-polyfill`](http://babeljs.io/docs/usage/polyfill/). This last option is also valid and convenient for command-line scripts, as that code is not meant to be embedded/required.

Finally, if polyfilling is not for you, just specify what is the minimum version of Node.JS needed to run your package by setting the `engines.node` property in the `package.json`:

```json
{
  "engines": { "node": ">=4.0.0" }
}
```

### Mocha, Chai and istanbul/nyc

Enjoy having it all ready to run the tests with Mocha, make them fail with Chai and analyze coverage with istanbul/nyc!

## License

WTFPL
