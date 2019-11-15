## 恢复订单

加载由特定任务处理的文件是 Gulp 成为一个很好的工具。这些文件的加载顺序与文件夹中的顺序相同。这意味着如果您需要在生成的文件中按特定顺序输出文件，则必须进行一些操作。可能发生这种情况的是一堆 CSS 文件的串联。在这样的文件中，顺序非常重要，因为它可能会使您的应用程序看起来与您的预期不同。

取以下三个小.css 文件和 Gulp 文件：

代码清单 42：/Assets/css/anotherstylewhichshouldbeattheend.css

```
div {
      border:4px solid red;
}

```

代码 43：/Assets/css/something.css

```
body {
      background-color: blue;
}

```

代码清单 44：/Assets/css/styles.css

```
div {
      border:1px dashed green;
      background-color: green;
}

```

代码 45：/gulpfile.js

```
var gulp = require('gulp'),

  concat = require('gulp-concat');

gulp.task('css', function () {

  return gulp.src('./Assets/css/**/*.css')
                        .pipe(concat('all.css'))
                        .pipe(gulp.dest('./wwwroot/css'));
});

gulp.task('default', ['css']);

```

我们在这里介绍了另一个 Gulp 插件： gulp-concat 。在运行 gulp 命令之前一定要安装它。它的作用是将流连接成一个文件，在这种特殊情况下为 all.css 文件。这使得将脚本捆绑在一起非常容易，并减少了浏览器需要获取的文件数量。

运行默认 Gulp 任务的输出将生成以下 all.css 文件：

代码 46：/wwwroot/css/all.css

```
div{
      border:4px solid red;
}
body {
      background-color: blue;
}
div {
      border:1px dashed green;
      background-color: green;
}

```

这不是我们想要的，因为使 div 边框变为红色的样式应该成为文件中的最后一个。我们可以用不同的方式做到这一点;让我们来研究一下。

### 通过 gulp.src 订购

这是一个非常简单的方法，因为您不必再​​获取另一个 Gulp 插件。实际上，它已经在您的代码中作为 gulp.src 。而不是简单地填充像 ./Assets/less/**/*.less 这样的全局来抓取 Assets / less 及其子文件夹下的所有 Less 文件，可以创建一个带有顺序的数组你想要他们的文件。让我们举几个例子：

代码 47：gulpfile.js，在 gulp.src 中有排序 - /gulpfile.js

```
var gulp = require('gulp'),
      concat
  = require('gulp-concat');

gulp.task('css', function () {

  return gulp.src(['./Assets/css/styles.css', './Assets/css/**/*.css'])
                        .pipe(concat('all.css'))
                        .pipe(gulp.dest('./wwwroot/css'));
});

gulp.task('default', ['css']);

```

一定要安装软件包 gulp ， gulp-concat 和 gulp-order 。

运行默认 Gulp 任务后，输出现在将变为以下内容：

代码 48：/wwwroot/css/all.css

```
div {
      border:1px dashed green;
      background-color: green;
}
div{
      border:4px solid red;
}
body {
      background-color: blue;
}

```

styles.css 文件在排序中作为第一个放置，并且在文件夹中按文件名顺序保留其余文件时，会生成 all.css 文件。

### 订购 gulp-order

即使通过 gulp.src 进行排序，它也很容易改变，实际上做了两件事：获取文件并对它们进行排序。从关注点分离的角度来看，并不总是希望这样。因此，最好使用专用的 Gulp 插件： gulp-order ，，它将在以下更改的 Gulp 文件中引入。

代码 49：gulpfile.js，通过 gulp-order 插件进行排序

```
var gulp = require('gulp'),
      order
  = require('gulp-order'),
      concat
  = require('gulp-concat');

gulp.task('css', function () {

  return gulp.src('./Assets/css/**/*.css')
                        .pipe(order(['styles.css', '*.css']))
                        .pipe(concat('all.css'))
                        .pipe(gulp.dest('./wwwroot/css'));
});

gulp.task('default', ['css']);

```

输出与代码清单 48 中的相同。