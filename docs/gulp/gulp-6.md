  

### gulp.task（）

这形成了 .src（）的逻辑包装。 dest（）和流。组成 Gulp 文件时，可以定义多个任务，甚至可以在某个任务运行之前定义依赖项。从一个文件夹获取文件并将其复制到另一个文件夹的一个非常简单的任务可能如下：

代码清单 2

```
gulp.task('copyScripts', function () {

  // copy any javascript
  files in source/ to public/

  gulp.src('source/*.js').pipe(gulp.dest('public'));
});

```

运行时，代码的功能如下：源文件夹中带扩展名的文件。 **js** 将被复制到 **public** 文件夹中。我们将在下一章中看到如何运行它。

### gulp.watch（）

此功能可以监视文件更改并相应地执行操作。 [第 3 章](../Text/gulp-19.html#_Chapter_3_Watching)将完全致力于此。