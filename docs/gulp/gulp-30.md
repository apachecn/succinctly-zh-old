## 脚本翻译

在过去几年中，网络的 JavaScript 使用量出现了令人难以置信的增长。在开始时，它仅用于响应某些点击事件以切换某些 HTML 元素（如图片）的可见性。如今，查看具有数千行 JavaScript 代码的 Web 应用程序，在浏览器中执行一系列任务而不是将所有内容发回服务器并等待答案也不例外。所谓的单一页面应用程序（SPA）的受欢迎程度越来越高。但它有一个缺点：复杂性变得非常强硬，很容易出现小错误。因此，我们也看到越来越多的工具和语言 _ 将您编写的代码 _ 转换为 JavaScript。通常这是一个手动任务，一段时间后可能会变得单调乏味。幸运的是，我们正在使用 gulp.js，它通过自动化操作使生活更轻松。

| ![](img/00021.gif) | 注意：Transpiling 是一个特定的术语，用于表示将一种语言转换为另一种语言的过程，同时保持相同的抽象级别 - 基本上从 CoffeeScript，TypeScript ...到 JavaScript。编译来自一种语言并将其转换为其他语言，例如将 C＃编译为二进制代码（在本例中为 IL）。 |

### CoffeeScript

CoffeeScript 可能是我遇到的第一种在翻译后生成 JavaScript 的语言。但是，如果你有一堆文件，并且有几个文件被更改，那就意味着逐个翻译它们。不再是 Gulp 了。有一个简单的插件可供我们使用，我们可以在我们的 gulp 任务或观察者中使用。

代码 23：CoffeeScript 中的 Fibonacci - /Assets/Coffee/fibonacci.coffee

```
fib
  = (x) ->

  if x < 2

  x

  else

  fib(x-1) + fib(x-2)

solutions
  = []
for number in [0..10]

  solutions.push ( fib number )

console.log
  solutions

```

样本是众所周知的 [Fibonacci 序列](https://en.wikipedia.org/wiki/Fibonacci)，我们希望将其翻译成 JavaScript。

在 **Fibonacci** 文件夹下直接创建一个新的 **gulpfile.js** ，然后运行以下命令：

1.  npm init （这将在您完成向导后生成 package.json 文件。）
2.  npm install gulp --save-dev
3.  npm install gulp-coffee --save-dev

在 **gulpfile.js** 文件中，输入以下内容：

代码 24：用于 CoffeeScript 演示的 gulpfile.js - /gulpfile.js

```
"use
  strict";

var gulp = require('gulp'),
      coffee
  = require('gulp-coffee');

gulp.task('coffee', function () {

  gulp.src('./Assets/Coffee/**/*.coffee')
            .pipe(coffee())
            .pipe(gulp.dest('./wwwroot/scripts'));
});

gulp.task('default', ['coffee']);

```

在终端窗口或 DOS 框中，运行默认的 Gulp 任务，该任务将按预期在 wwwroot / scripts 文件夹下输出以下 JavaScript 文件（如 gulp.dest 的参数所指定）：