  

代码 67：使用 gulp-inject 注入三个.css 文件后的结果

```
<head>

  <meta charset="utf-8" />

  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>@ViewData["Title"] - GulpInVSWatch</title>

  <!-- inject:css
  -->

  <link rel="stylesheet" href="/wwwroot/css/AnotherOne.css">

  <link rel="stylesheet" href="/wwwroot/css/site.css">

  <link rel="stylesheet" href="/wwwroot/css/Styles.css">

  <!-- endinject
  -->

</head>

```

这很好用，但这不是我们希望的完整体验。还记得我们在段落前面谈到的版本控制条件吗？那么，是时候解决这个问题了。幸运的是，不需要另一个 Gulp 插件。 gulp-inject 插件有很多选项，我们可以利用和操纵注入的内容。

在终端窗口的 DOS 框中，我们现在将执行 npm install gulp-inject --save-dev 命令从 npm 获取包，以便我们可以使用它。在 Visual Studio 中，它有点不同。

打开 **package.json** 文件并进行编辑。 Visual Studio 的优点在于它在编辑 package.json 文件时也提供了 IntelliSense。在 devDependencies 部分，为 gulp-inject 添加一个新行。在 Solution Explorer 中，右键单击 **npm** 节点并选择 **Restore packages** 。

打开 **package.json** 文件，如图 45 所示。您可以为 gulp-inject 添加额外的行，并获取包名称和版本的 IntelliSense。现在这很方便，不是吗？

![](img/00049.jpeg)

_ 图 45：编辑 package.json 时的 IntelliSense_

很可能你已经获得了自动加载 npm 包的 Visual Studio 更新。如果你没有，那么你可以采取下一个简单的步骤，如图 46 所示：

![](img/00050.jpeg)

_ 图 46：在 Visual Studio 中恢复包 _

相应地将 gulpfile.js 文件更改为：

代码 68：gulpfile.js 用于转换注入的文件并添加查询字符串

```
var gulp = require("gulp"),

  inject = require('gulp-inject');

gulp.task("inject", function () {

  var target = gulp.src('./Views/Shared/_layout.cshtml');

  var sources = gulp.src('./wwwroot/css/**/*.css');

  var ticks = new Date().getTime();

  return target

  .pipe(inject(sources, {

  transform: function (filepath) {

  arguments[0] = filepath + '?v=' +
  ticks;

  return
  inject.transform.apply(inject.transform, arguments);

  }

  }))

  .pipe(gulp.dest('./Views/Shared/'));
});

```

这里我们添加了一个额外的选项来转换文件路径。它的后缀是？v = 以及我们在转换之前用当前日期的滴答数填充的滴答数。注入后，最终结果将与以前略有不同：

代码 69：使用 gulp-inject 和添加的查询字符串注入三个.css 文件后的结果

```
<head>

  <meta charset="utf-8" />

  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>@ViewData["Title"] - GulpInVSWatch</title>

  <!-- inject:css
  -->

  <link rel="stylesheet" href="/wwwroot/css/AnotherOne.css?v=1448220822909">

  <link rel="stylesheet" href="/wwwroot/css/site.css?v=1448220822909">

  <link rel="stylesheet" href="/wwwroot/css/Styles.css?v=1448220822909">

  <!-- endinject
  -->

</head>

```

这导致了我们从一开始就想要的东西。由于 gulp-inject 插件的转换选项，操作注入文件名的结果变得非常容易。现在将它与 Visual Studio 的 Task Runner Explorer 中的 After Build 绑定结合起来，您就拥有了可靠的开发人员体验。