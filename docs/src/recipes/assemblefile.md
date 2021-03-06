# example assemblefile.js

The following basic `assemblefile.js` includes tasks for generating:

- `.html` files from `.hbs` ([handlebars][]) templates 
- `.css` stylesheets from `.less` ([less][])

```js
var assemble = require('assemble');
var extname = require('gulp-extname');
var less = require('gulp-less');
var app = assemble();

app.task('html', function() {
  app.src('templates/*.hbs')
    .pipe(app.renderFile())
    .pipe(extname('.html'))
    .pipe(app.dest('dist/'));
});

app.task('css', function () {
  app.src('styles/*.less')
    .pipe(less())
    .pipe(app.dest('dist/assets/css'));
});

app.task('default', ['html', 'css']);
```

{%= reflinks(['less', 'handlebars']) %}