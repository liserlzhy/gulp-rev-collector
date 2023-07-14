> Relative path processing is added based on the gulp-rev-collector-dxb

```
---example

x.js             ->    x.js?v=b9213618a6
/xx/x.js         ->    /xx/x.js?v=b9213618a6
./xx/x.js        ->    ./xx/x.js?v=b9213618a6
../../xx/x.js    ->    ../../xx/x.js?v=b9213618a6
```

## Install

```sh
$ npm install --save @liserl/gulp-rev-collector-dxb
```

## Usage

```js
var gulp = require('gulp');
var rev = require('gulp-rev-dxb');

gulp.task('css', function () {
    return gulp.src('src/css/*.css')
        .pipe(rev())
        .pipe(gulp.dest('dist/css'))
        .pipe( rev.manifest() )
        .pipe( gulp.dest( 'rev/css' ) );
});

gulp.task('scripts', function () {
    return gulp.src('src/js/*.js')
        .pipe(rev())
        .pipe(gulp.dest('dist/js'))
        .pipe( rev.manifest() )
        .pipe( gulp.dest( 'rev/js' ) );
});

...

var revCollector = require('@liserl/gulp-rev-collector-dxb');
var minifyHTML   = require('gulp-minify-html');

gulp.task('rev', function () {
    return gulp.src(['rev/**/*.json', 'templates/**/*.html'])
        .pipe( revCollector({
            replaceReved: true,
            dirReplacements: {
                'css': '/dist/css',
                '/js/': '/dist/js/',
                'cdn/': function(manifest_value) {
                    return '//cdn' + (Math.floor(Math.random() * 9) + 1) + '.' + 'exsample.dot' + '/img/' + manifest_value;
                }
            }
        }) )
        .pipe( minifyHTML({
                empty:true,
                spare:true
            }) )
        .pipe( gulp.dest('dist') );
});
```
