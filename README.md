# TGAM Flexbox Grid

## Introduction

A mobile-first flexbox grid system based on the CSS flex display property. A customized, name-spaced, and extended Bootstrap 4 grid.

It also supports different column counts for each of the 4 breakpoint tiers. Which are extra-small (1), small (8), medium (12), and large (16).
[Check the documentation](http://globeandmail.github.io/tgam-flexboxgrid).

This project is an extension of our living style guide. It uses [Gulp](http://gulpjs.com/) tasks to generate a [static documentation site](https://github.com/blog/2228-simpler-github-pages-publishing), which then gets [deployed](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/) to a Github. It also doubles as an [NPM package](https://www.npmjs.com/). `npm install` it into your projects and you'll be able to `@import` SCSS from the `dist` directory.

## Installation

1. If you haven't already done so, download and install [Node.js](https://nodejs.org/) (which includes the [NPM command-line tool](https://www.npmjs.com/)).
2. Open your terminal and clone this repository.
3. Navigate to the project directory.
4. Run `npm install`.
5. Run `./node_modules/.bin/gulp`.
6. The website should automatically open in Google Chrome, with this URL: `http://localhost:3003`.

## Codebase overview

The codebase is divided into the following directories:

- `dist`: Contains a distribution-ready copy of the flexbox grid style files. Outside projects will then be able to install as a dependency.
- `gulp`: Contains all of the [Gulp](http://gulpjs.com/) tasks that are responsible for generating the static website and copying the project files into the various folders.
- `public`: Contains the generated static website files. The contents of this directory will be deployed to a web server.
- `src`: Contains all of the "working files". You'll spend most of your time inside of this directory.
  - `patterns`: Contains the actual flexbox grid files (which will be automatically copied into the `dist` directory).
  - `specimens`: Contains examples of the grid in action, which will serve as living documentation on the static website.
  - `site`: Files in this directory define the structure, behaviour and visual style of the static website. This is where you'll go to add pages and navigation links.

### Workflow

The examples in the `specimens` directory serve as living documentation for how flexbox grid in the library are intended to be used. Each specimen a separate HBS file and will not appear on the static website until you've manually added it to a page (see the "Creating pages" section, below).

### Getting started

Start by creating a new file inside of `specimens`, with the following (empty) files inside of it:

```
src
  specimens
    example
      my-example.hbs
```

### Handlebars partials

For each specimen in the `src/specimens` directory, two [Handlebars](http://handlebarsjs.com/) partials will automatically be registered for you. For example, if your specimen lives inside a directory named `src/specimens/example`, then the following Handlebars partials should be available for you to use:

1. `{{> example/my-example }}`
2. `{{> example/my-example-code }}`

These are essentially just file includes ([read more about partials here](http://handlebarsjs.com/partials.html)).

The first partial will contain the specimen's HTML markup, and is intended to be used primarily inside of a page. These specimens cannot be composed (nested) together.

The second partial will contain an HTML code that's intended to be displayed on pages in the static website. The code will include markup that is to be styled, formatted, and readable documentation.

## Previewing your work

In order to preview your work, you'll need to compile the static website by running the Gulp task (`./node_modules/.bin/gulp`). While it's running, Gulp should monitor your files for changes and recompile your SASS as required. It will also start up a local web server, which will make it possible to view the static website by navigating to http://localhost:3003 in your web browser.

If you encounter a situation where Gulp isn't recompiling properly, then just stop it via <kbd>control + c</kbd> (on your keyboard) and restart it via `./node_modules/.bin/gulp` (in your terminal). Please note that Gulp is only able to monitor the files that existed when its `watch` tasks started up, so you'll need to restart it when you create new files and directories.

## Adding links to the menu

To add a new link to the static website's navigation menu, edit the `src/site/navigation.yml` file. This file defines the structure of the menu, in [YAML](http://www.yaml.org/start.html) format. It should look something like this:

```
primaryNav:
  -
    name: "Home"
    path: "/"
  -
    name: "Another page"
    path: "/another-page.html"
  -
    name: "External link"
    path: "http://www.google.com"
  -
    name: "Some category"
    path: "/some-category"
    children:
      -
        name: "Nested page"
        path: "/some-category/nested-page"
```

The above menu structure will generate HTML similar to this:

```
<ul>
  <li><a href="/">Home</a></li>
  <li><a href="/another-page.html">Another page</a></li>
  <li><a href="http://www.google.com">External link</a></li>
  <li>
    <a href="/some-category">Some category</a>
    <ul>
      <li><a href="/some-category/nested-page">Nested page</a></li>
    </ul>
  </li>
</ul>
```

**Note**: Only one level of link nesting is currently supported, for the left navigation. Also, the homepage will only show 4 levels of link nesting.

## Working with the library files

The library consists of the SCSS that live inside of the `src/patterns/flexboxgrid` directory:

```
src/patterns/flexboxgrid
```

These are the files that get copied into the `dist` directory when the Gulp tasks run, and get published when you deploy the static website files (see below).

## Releasing new versions of the flexbox grid library

In order to release a new version of the flexbox grid library, you'll need to do the following:

- Run `git checkout master` branch.
- Manually increment the NPM package's version number in `package.json` via `npm version patch`.
- That will automatically create a commit of the `package.json` file, titled with the new version number.
- Re-generate the `docs` and `dist` folders for Github pages via the command `gulp publish`.
- Commit the generated files into master via the command `git add -A` and `git commit -m "Latest publish"`.
- Run `git push origin\master` branch.
- Republish the NPM package to the NPM registry via `npm publish` (you may need to login first).

### Development workflow

It's best practice to create a new Git branch before you start making changes to the codebase:

1. Check out the `master` branch: `git checkout master`.
2. Pull down the latest changes: `git pull`.
3. Create a new working branch: `git checkout -b my-new-branch`.
4. Push your local branch up to the remote repository: `git push -u origin my-new-branch`.
5. Do some work and commit your changes: `git commit -m "My commit message"`.
6. Repeat step 5 as many times as necessary.
7. When you're finished working, go to Github.com and [create a new pull request](https://github.com/globeandmail/tgam-flexboxgrid/pulls).
8. Wait for your peers to review your pull request and merge it into `master`.
9. Follow the above instructions to publish a new version of the flexbox grid library to Github pages.

## Importing the flexbox grid library NPM package into outside projects

In order to make use of the library, you'll need to install it as a dependency in your project(s).

1. If you haven't already done so, install a task runner such as [Gulp](http://gulpjs.com/) (recommended).
2. If you haven't already done so, initialize your project via `npm init`.
3. Install the library module via `npm install --save-dev tgam-flexboxgrid`.
4. Create a file called `gulpfile.js` (or edit your existing `gulpfile.js`) and add the following:

```
var gulp = require("gulp");
var sass = require("gulp-sass");
var tgamFlexboxgrid = require('tgam-flexboxgrid').sassIncludePath;

// Compile SASS files

gulp.task("styles:compile", function() {
  return gulp.src("./path/to/your/main/stylesheet.scss")
    .pipe(sass({
      includePaths: tgamFlexboxgrid // SASS include paths
    })
    .pipe(gulp.dest("./path/to/your/output/directory"));
});
```

You should now be able to import the library into your SCSS files via `@import "flexboxgrid/all";` and compile them via `gulp styles:compile`.

```
// Before importing library, set any default/custom SASS variables here, based on: flexboxgrid/_variables.scss
$grid-gutter-width-base: 30px; // e.g. instead of 20px
// Also see documentation regarding excluding grid features, by setting SASS vars here.
@import "flexboxgrid/all";
```

### Updating the NPM package

1. Run `npm outdated` in your command line to see if a newer version of the library is available.
2. Edit `package.json` and manually increment the `tgam-flexboxgrid` version number to the latest.
3. Run `npm update tgam-flexboxgrid`.
4. Double check that the `node_modules/tgam-flexboxgrid/dist` directory exists. If not, then someone forgot to run the Gulp tasks before publishing this version of the pattern library.

**Note**: Unfortunately, it's necessary to manually edit package.json because running `npm-update` alone will not update the version number in your `package.json` file, nor will running `npm install --save tgam-flexboxgrid`.

### Referencing a Git branch instead of an NPM package version

During development, you may find it useful to import the pattern library files from a Git branch instead of from the NPM registry. This enables you to commit and push code to your `tgam-flexboxgrid` branch, then immediately pull it into your outside project, without having to publish a new version to NPM. This is ideal because you don't want to publish an official release version to NPM until your code is production-ready.

**Example of pulling in a Git branch** (which you would do during development):

```
"dependencies": { "tgam-flexboxgrid": "git://github.com/globeandmail/tgam-flexboxgrid.git#branch-name" }
```

Once you've finished your work and your code is production-ready, you should then merge your `branch-name` branch into `master`, publish a new version to NPM, then point the `tgam-flexboxgrid` dependency in your consuming project's `package.json` back to the NPM registry once again.

**Example of pulling in a version of the module from NPM** (which you should do in production):

```
"dependencies": { "tgam-flexboxgrid": "1.0.3" }
```

## Inspiration

- [Bootstrap 4](http://v4-alpha.getbootstrap.com) (v4-alpha) created by @mdo and @fat.
- [Flexbox Grid Sass](http://hugeinc.github.io/flexboxgrid-sass/) created by @hugeinc.
- [Flexbox Grid](http://flexboxgrid.com/) created by @kristoferjoseph.
