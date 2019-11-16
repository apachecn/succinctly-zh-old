## 在 Visual Studio 中观察 Gulp 的变化

我们在前一段中看到的内容可以用来回答我在 [Web 欧洲大会 2015](http://webnextconf.eu/) 期间提出的问题，同时介绍 Gulp：_ 如何在使用 Visual 时更改文件并运行任务工作室？_

通过以下一些简单的步骤可以看出答案。我们将使用 gulp-sass 将 Sass 文件转换为相应的 CSS 文件。为此，在 **wwwroot / css /下创建两个 Sass 文件。**

代码 61：AnotherOne.scss

```
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;

  -moz-border-radius: $radius;

  -ms-border-radius: $radius;

  border-radius: $radius;
}

.box { @include border-radius(10px);
  }

```

代码 62：Styles.scss

```
@import "AnotherOne.scss";

a.CoolLink {

  color: greenyellow;

  &:hover {

  text-decoration: underline;

  }

  &:visited {

  color: green;

  }
}

```

代码 63：gulpfile.js

```
///
  <binding Clean='clean' ProjectOpened='watch' />

var gulp = require("gulp"),

  sass = require('gulp-sass'),

  project = require("./project.json");

var paths = {

  webroot: "./" + project.webroot + "/"
};

paths.js
  = paths.webroot + "js/**/*.js";
paths.sass
  = paths.webroot + "css/**/*.scss";
paths.sassToCss
  = paths.webroot + "css";

gulp.task("css:sass", function () {

  gulp.src(paths.sass)

  .pipe(sass())

  .pipe(gulp.dest(paths.sassToCss));
});

gulp.task('watch', function () {

  gulp.watch(paths.sass, ['css:sass']);
});

```

几乎整理了 gulpfile.js 文件以显示完成任务的方法。需要需要语句并设置路径。创建了两个任务：一个 css：sass ，用于将.scss 文件转换为.css 文件，以及监视任务。每当它看到 wwwroot / css 文件夹下的某个.scss 文件发生变化时，就会运行 css：sass 任务。

这不是什么新东西，因为我们已经在第 3 章中讨论了类似的方法。新的部分是 Visual Studio 的反应方式。在 gulpfile.js 文件的顶部，您可以看到以下行：

///&lt; binding Clean ='clean'ProjectOpened ='watch'/&gt;

第一部分是熟悉的，正如我们在前一个例子中看到的那样。使用前面讨论的工具， watch 任务已经与 Task Runner Explorer 中的 Project Open 绑定相结合，如图 43 所示。

![](img/00047.jpeg)

图 43：Gulp 任务监视与 Project Open 绑定相结合

您现在可以手动启动手表任务或关闭 Visual Studio。再次打开 Visual Studio 并重新打开该项目。打开项目后，直接查看 Task Runner Explorer。您将看到手表任务已运行。现在，每当您更改 wwwroot / css 文件夹下的.scss 文件时，保存更改的文件时将运行 css：sass 任务。图 44 显示了 Task Runner Explorer 窗格中的输出。

![](img/00048.jpeg)

图 44：Watch 项目在打开项目时运行，并在每次保存.scss 文件后进行更改。

现在我们已经实现了与使用简单控制台窗口时相同的功能，就像在第 3 章中一样。这使我们在 Visual Studio 中的开发工作更加愉快。

某些更改后 Styles.css 的结果可能如下所示：