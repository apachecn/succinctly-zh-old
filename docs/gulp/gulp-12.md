  

代码 4：/Assets/Styles.less

```
@import "Colors.less";

body {

  background-color: @backcolor;
}

a {

  color: @color;

  &:hover {

  color: @color + @backcolor;

  }
}

```

现在直接在**第 2 章**文件夹下，打开 **gulpfile.js** 文件并添加以下代码：

代码 5：/gulpfile.js

```
"use
  strict";

var gulp = require('gulp');
var less = require('gulp-less');
var minifyCSS = require('gulp-clean-css');

gulp.task('default', function () {

  gulp.src('Assets/Styles.less')

  .pipe(less())

  .pipe(gulp.dest('wwwroot/css'));
});

```

在代码清单 5 中，我们看到需要一些插件，所以让我们通过命令安装它们。务必将终端窗口或控制台窗口的路径设置为第 2 章：

1.  npm install --save-dev gulp
2.  npm install --save-dev gulp-less
3.  npm install --save-dev gulp-clean-css

| ![](img/00005.jpeg) | 提示：当你不在 Mac 上以管理员权限运行时，可以在 sudo 前加上这些命令的前缀，因此它变为 sudo npm install --save-dev gulp 。这将要求输入密码，之后安装将继续，而不必使用管理权限运行。 |

从您现在所在的终端窗口或 DOS 窗口，确保您位于 gulpfile.js 文件所在的文件夹第 2 章。输入 gulp 并按**确认**键。稍等一下，看看新创建的 wwwroot \ css 子文件夹下的结果。根据您的机器，这可以是快速或非常快 - 伟大的性能是 Gulp 的优势之一。

![](img/00007.jpeg)

图 4：在 Gulp 中运行默认任务

现在只有一个 styles.css 文件，具有以下预期输出：

代码 6：/wwwroot/css/styles.css

```
body {
  background-color: #808080;
}
a {
  color: #b6ff00;
}
a:hover {
  color: #ffff80;
}

```

很高兴看到我们的少文件被编译并写出来了。我们可以使用 **css** 文件。但我们也想缩小.css 文件，所以重新打开 **gulpfile.js** 文件并使用 minification 插件，如下所示：