  

代码 65：_Layout.cshtml 头部

```
<head>

  <meta charset="utf-8" />

  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>@ViewData["Title"] - GulpInVSWatch</title>

  <!-- inject:css
  -->

  <!-- endinject
  -->

</head>

```

为了使它工作，我们需要编写我们的 gulpfile.js 文件，如下面的代码片段：

代码 66：gulp-inject 的 gulpfile.js

```
var gulp = require("gulp"),

  inject = require('gulp-inject');

gulp.task("inject", function () {

  var target = gulp.src('./Views/Shared/_layout.cshtml');

  var sources = gulp.src('./wwwroot/css/**/*.css');

  return target

  .pipe(inject(sources))

  .pipe(gulp.dest('./Views/Shared/'));
});

```

这小段 Gulp 代码在注入任务中发挥了神奇作用。它抓取了目标文件，在我们的例子中是 _Layout.cshtml。在这种情况下，源代码将是我们可以在子文件夹 wwwroot / css 中找到的所有.css 文件，并将放在 _Layout.cshtml 文件的 head 部分中特殊组成的注释行之间。

接下来的行获取目标文件，注入源并将更改后的 _Layout.cshtml 文件写回到 Views / Shared /下的原始位置。

可以在以下代码清单中看到此操作的结果。在示例解决方案中，我有三个不同的.css 文件，它们全部注入：