var gulp = require('gulp');
var sass = require("gulp-sass");
var browserSync = require('browser-sync').create();
var pkg = require('./package.json');
 
// Copy third party libraries from /node_modules into /vendor
gulp.task('vendor', function() {

  // Bootstrap
  gulp.src([
      './node_modules/bootstrap/dist/**/*',
      '!./node_modules/bootstrap/dist/css/bootstrap-grid*',
      '!./node_modules/bootstrap/dist/css/bootstrap-reboot*'
    ])
    .pipe(gulp.dest('./vendor/bootstrap'))

  // jQuery
  gulp.src([
      './node_modules/jquery/dist/*',
      '!./node_modules/jquery/dist/core.js'
    ])
    .pipe(gulp.dest('./vendor/jquery'))
})

//sass watch
gulp.task('sass', function () {  
  gulp.src('./sass/*.scss')
      .pipe(sass({includePaths: ['scss']}))
      .pipe(gulp.dest('./public/css'));
});

// Default task
gulp.task('default', ['vendor']);

// Configure the browserSync task
gulp.task('browserSync', function() {
  browserSync.init({
    server: {
      baseDir: "./"
    }
  });
});

// Dev task
gulp.task('dev', ['sass', 'browserSync'], function() {
  gulp.watch('./sass/*.scss', ['sass', browserSync.reload]);
  gulp.watch('./*.html', browserSync.reload);
});

