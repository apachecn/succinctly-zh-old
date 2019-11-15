## Gulp 如何运行任务

### Orchestrator

在 Gulp 3 中，我们注意到我们可以轻松地编写任务依赖关系，如下所示： [ **' css：less '，'脚本：打字稿']** 。这将导致那些依赖任务以最大可能的并发性运行。在幕后，Gulp 将依次创建一个依赖树，并按顺序编排任务的确切运行。代码清单 12 显示了这一点。为了连续阅读，我们将重复该列表：

代码 12：从不同的地方运行任务

```
"use
  strict";

var gulp = require('gulp');

gulp.task('clean', function () {

  console.log('Cleaning
  up...');
});

gulp.task('task1', ['clean'], function () {

  console.log('Task 1
  is executing...');
});

gulp.task('task2', ['clean'], function () {

  console.log('Task 2
  is doing its thing...');
});

gulp.task('build', ['task1', 'task2']);

gulp.task('default', ['build'], function () {

  console.log('default
  task...');
});

```

clean 任务只执行一次，即使它被引用为 task1 和 task2 的依赖任务。 Orchestrator 确保它发生这样，并且 clean 任务不会被调用两次。如果它被调用两次，甚至更多，那么某些任务可能会通过清除已经使清除任务运行的另一个任务的结果而造成严重破坏。这是你不想发生的情况。

### 系列和平行线

随着 Orchestrator 的使用，事物将按照它构成依赖树的顺序运行，但这可能感觉有点像魔术。如果没有你对它的控制，事情就会继续。由于依赖任务的使用，你可以给 Gulp 提示以某种方式运行，但仍然感觉有点像你没有一切都在控制之下。

Gulp 4 解决了这个问题。因为人们想要更多地控制将要运行的内容以及何时运行。因此，Orchestrator 已被搁置，并引入了两个新的执行功能：

*   Gulp.series ：用于顺序执行
*   Gulp.parallel ：用于并行执行

两者都接受以下参数：

*   要执行的任务名称
*   要执行的功能

通过组合这些，您可以根据需要编制复杂的执行订单。但是，请注意，保持简单仍然是一件好事，因为过于复杂的系统往往难以调试和维护。

之前我们在 Gulp 4 中提到，任务 API 看起来会有所不同： gulp.task（name，fn）。

fn 可以是串联，并联，串联和并联的组合，也可以是函数。

编写要并行运行的任务，例如将 Less 转换为 CSS 和将 TypeScript 转换为 JavaScript，可能如下所示：

代码 70：Gulp.parallel

```
gulp.task('default', gulp.parallel('css:less', 'js:typescript'));

```

再看看代码清单 12 并尝试使用新语法重写它，如下所示：

代码 71：代码清单 12 用 Gulp 4 重写

```
"use
  strict";

var gulp = require('gulp');

gulp.task('clean', function () {

  console.log('Cleaning
  up...');
});

gulp.task('task1', function () {
    console.log('Task 1 is executing...');
});

gulp.task('task2', function () {

  console.log('Task 2
  is doing its thing...');
});

gulp.task('build', gulp.series('clean', gulp.parallel('task1', 'task2')));

gulp.task('default', gulp.series('build'));

```

除了我们引入 gulp.series 和 gulp.parallel 函数调用之外，语法看起来几乎相同。需要注意的一件非常重要的事情是要求清理任务;它被作为 task1 和 task2 的依赖关系。允许这两个并行运行，但与 Gulp 4 一样，我们不再使用 Orchestrator。这将导致清理任务运行两次，我们需要不惜一切代价避免。

图表可以更直观的方式说明这一点。在并行调用之后添加了一个额外的系列步骤，以表明这也是可能的。

![](img/00051.jpeg)

图 47：Gulp 4 中的串行和并行任务