# plop-pack-rename-many

> plop helper for renaming multiple files via a glob pattern

## Motivation

My main issue was that I didn't want hidden/dot files (ex: .gitignore, .npmrc, .babelrc, .eslintrc, .gitlab-ci.yml, etc.) in my templates folder because they actually change how my repository functions in that directory.

Here's some examples:

1. My templates/.eslintrc file now causes many of my javascript files in my templates directory to have linting errors because they have handlebars syntax for injecting data.
2. My templates/.gitignore file now ignores some of the template files that I actually don't want ignored any more.

So, I wrote this custom `renameMany` action (`plop-pack-rename-many`).

Not only that, but the renaming action can be used for things other than just hidden/dot files.

## Example

```javascript
// plopfile.js

module.exports = (plop) => {
  plop.load('plop-pack-rename-many')
  plop.setGenerator('generatory-name-here', {
    actions: [
      {
        type: 'addMany',
        destination: process.cwd(),
        templateFiles: './templates/**/*',
        globOptions: { dot: true },
        base: './templates',
        force: true
      },
      { // converts filenames starting with an underscore to dot files.
        type: 'renameMany', // <-- the plop-pack-rename-many action!
        templateFiles: `${process.cwd()}/**/_*`, // glob pattern to select all files starting with an underscore.
        renamer: name => `.${name.slice(1)}` // rename by removing the underscore and add the period.
      }
    ]
  })
}
```
