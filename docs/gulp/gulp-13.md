  

代码清单 7：调整后的/gulpfile.js

```
"use
  strict";

var gulp = require('gulp');
var less = require('gulp-less');
var minifyCSS = require('gulp-clean-css');

gulp.task('default', function () {

  gulp.src('Assets/Styles.less')

  .pipe(less())

  .pipe(minifyCSS({ keepBreaks: false }))

  .pipe(gulp.dest('wwwroot/css'));
});

```

再次运行 Gulp，现在结果变为：

代码 8：缩小的 css 文件：/wwwroot/css/styles.css

```
 body{background-color:grey}a{color:#b6ff00}a:hover{color:#ffff80}

```

### 选项

在代码清单 6 中，我们不仅使用了对 minifyCSS 的调用，而且还传入了一个参数 {keepBreaks：false} 。一些插件提供传递选项的能力。根据您使用的文本编辑器，您可能会获得代码完成，但通常最简单的方法是查看文档。例如，对于 gulp-clean-css ，我们可以看一下[这个文档](https://www.npmjs.com/package/gulp-clean-css)。

但是，我们没有看到提到的选项。怎么会？我们可以在该页面上看到这个插件是 clean-css Node.js 库的简单包装器。进一步单击该特定库的文档，我们可以看到[概述](https://github.com/jakubpawlowicz/clean-css)。如你所见，它们中有很多。

作为练习，您可以使用不同的选项，看看输出会发生什么。您可以从 Gulp 文件中最简单的更改开始，然后设置 {keepBreaks：true} 。