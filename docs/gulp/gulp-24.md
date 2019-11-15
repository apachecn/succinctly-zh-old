  

代码 17：使用 gulp-watch 插件/gulpfile.js

```
"use
  strict";

var gulp = require('gulp');
var less = require('gulp-less');
var gulpWatch = require('gulp-watch');

var lessPath = './Assets/**/*.less';

gulp.task('lessToCss', function () {

  gulp.src(lessPath)

  .pipe(gulpWatch(lessPath))

  .pipe(less())

  .pipe(gulp.dest('wwwroot/css'));
});

gulp.task('default', ['lessToCss']);

```

现在运行 Gulp 默认任务并尝试在 **/ Assets** 文件夹中添加一个新的.less 文件。您会注意到在 wwwroot / css 文件夹中会有一个新的.css 文件。