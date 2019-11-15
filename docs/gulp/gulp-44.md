## 动态加载插件

到目前为止，我们已经在每个 Gulp 文件的顶部编写了 require 语句。当您添加大量插件时，该列表可能会变得很长。例如，以下不是例外：

代码 53：许多 require 语句 - /gulpfile.js

```
var gulp = require('gulp'),

  del = require('del'),

  less = require('gulp-less'),

  path = require('path'),

  autoprefixer = require('gulp-autoprefixer'),

  sourcemaps = require('gulp-sourcemaps'),

  concat = require('gulp-concat'),

  order = require('gulp-order'),

  filesize = require('gulp-filesize'),

  uglify = require('gulp-uglify'),

  rename = require('gulp-rename'),

  minify = require('gulp-minify'),

  connect = require('gulp-connect'),

  jshint = require('gulp-jshint'),

  jade = require('gulp-jade'),

  minifyCss = require('gulp-clean-css'),

  coffee = require('gulp-coffee');

```

当你开始添加额外的插件时，这可能会变得更加麻烦。然而，有一种替代方法，通过使用 Gulp 插件极大地减少了这个列表。以下清单显示了 gulp-load-plugins 插件的用法。

代码 54：将它简化为 lazily 加载插件 - /gulpfile.js

```
var gulp = require('gulp'),

  gulpLoadPlugins = require('gulp-load-plugins'),

  plugins = gulpLoadPlugins();

```

秘诀在于插件在 package.json 文件中已知并通过 npm 安装。所以当我们有以下 package.json 时：

代码清单 55：/package.json

```
{
  "name": "d",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {

  "test": "echo \"Error: no test specified\"
  && exit 1"

  },
  "author": "",
  "license": "ISC",
  "devDependencies": {

  "gulp": "^3.9.0",

  "gulp-less": "^3.0.3",

  "gulp-load-plugins": "^0.10.0",

  "gulp-sass": "^2.0.4",

  "gulp-util": "^3.0.6"

  }
}

```

然后我们可以重构一个 gulpfile.js 文件来使用插件变量：

代码 56：gulpfile.js 以懒惰的方式加载插件 - /gulpfile.js

```
var gulp = require('gulp'),

  gulpLoadPlugins = require('gulp-load-plugins'),

  plugins = gulpLoadPlugins();

gulp.task('css:less', function () {

  return gulp.src('./b.less')
                        .pipe(plugins.less())
                        .pipe(gulp.dest('./'));
});

gulp.task('css:sass', function () {

  return gulp.src('./c.scss')
                        .pipe(plugins.sass())
                        .pipe(gulp.dest('./'));
});

gulp.task('default', function () {

  return plugins.util.log('Gulp is running!')
});

```

我们不是写 less（），而是编写 plugins.less（）以使其工作。