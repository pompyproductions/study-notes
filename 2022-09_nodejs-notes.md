# Node.js notes

## Global installations

```bash
$ npm i --global [packagename] # install
$ npm rm --global [packagename] # delete
```

- gulp-cli


## Gulp & Sass


### Setup

```bash
$ npm init
$ npm i -D gulp gulp-sass sass
```

Don't be alarmed by the vulnerability issues, just see them.

```js
// "gulpfile.js"
const { src, dest, watch, series } = require("gulp");
const sass = require("gulp-sass")(require("sass"));

// name this (build function) as you'd like
function buildStyles() {
    return src("src/sass/style.scss") // relative path to source
        .pipe(sass())
        .pipe(dest("dist")) // relative path to destination folder
}

// name this (watcher function) as you'd like
function watchTask() {
    watch(["src/**/*.scss"], buildStyles) 
}

exports.default = series(buildStyles, watchTask)
```

No need for any json scripts as long as you have gulp-cli installed. Just run `$ gulp` and it should be good to go.


### Folder structure

As specified in "gulpfile.js", the "dist" folder will be created automatically. The rest of it you'll have to create yourself:

```
(project root)
├── README.md
├── .gitignore    
├── .gulpfile.js
├── package.json
├── package-lock.json
├── src
│   └── sass
│       ├── style.scss
│       ├── abstracts
│       ├── base
│       ├── utilities
│       ├── components
│       ├── layouts
│       └── vendors
└── folder2
    │   file021.txt
    │   file022.txt
```