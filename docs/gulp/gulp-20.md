## 正在看文件

在开发您的网站或 Web 应用程序时，您知道在完成浏览器中的更改之前，您必须先执行一些手动操作。尽管 Gulp 已经让任务变得更轻松，但它仍然需要一些手动干预，比如打开一些 shell 并输入像 gulp generateCssFromLessFiles 这样的命令。

如果我们可以跳过这一部分并让 Gulp 自己解决问题，那不是很好吗？欢迎使用 Gulp watch API。

我们将开始简单并基于代码清单 5 的下一代码，我们在第 2 章中看到了它。它将被改变以实现我们的目标：减少重复性工作和自动化 Gulp 任务。在名为第 3 章的文件夹中，我将它放在与第 2 章文件夹相同的级别，创建了一个新的 **gulpfile.js** 文件，并将代码清单 11 中的代码放入其中。还要创建一个名为 **Assets** 的子文件夹，在其中放置 **.less** 文件，如下面的代码所示：

代码 13：查看文件：/gulpfile.js

```
"use
  strict";

var gulp = require('gulp');
var less = require('gulp-less');

gulp.task('watchLessFiles', function () {

  gulp.watch('./Assets/styles.less', function (event) {

  console.log('Watching
  file ' + event.path + ' being ' + event.type + ' by gulp.');

  })
});

gulp.task('default', ['watchLessFiles']);

```