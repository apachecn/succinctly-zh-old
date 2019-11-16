  

代码 30：编译的 JavaScript - /wwwroot/js/inheritance.js

```
var __extends = this.__extends || function
  (d, b) {

  for (var
  p in b) if
  (b.hasOwnProperty(p)) d[p] = b[p];

  function __() { this.constructor = d; }

  __.prototype = b.prototype;

  d.prototype = new __();
};
var Animal = (function () {

  function Animal(name) {

  this.name = name;

  }

  Animal.prototype.move = function (meters) {

  alert(this.name + " moved " + meters + "m.");

  };

  return Animal;
})();

var Snake = (function (_super) {

   __extends(Snake, _super);

  function Snake(name) {

  _super.call(this, name);

  }

  Snake.prototype.move = function () {

  alert("Slithering...");

  _super.prototype.move.call(this, 5);

  };

  return Snake;
})(Animal);

var Horse = (function (_super) {

  __extends(Horse, _super);

  function Horse(name) {

  _super.call(this, name);

  }

  Horse.prototype.move = function () {

  alert("Galloping...");

  _super.prototype.move.call(this, 45);

  };

  return Horse;
})(Animal);

var sam = new Snake("Sammy the Python");
var tom = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);

```

哇，我打赌你不想自己写。您也可以在 ts 调用中传入选项。请务必查看它们并与它们一起玩，看看哪些对您有用，例如源代码生成，例如：

代码 31：gulpfile.js，用于使用选项 - /gulpfile.js 编译 TypeScript

```
var
  gulp = require('gulp'),
      ts
  = require('gulp-typescript-compiler');

gulp.task('ts',
  function() {
      return
  gulp.src('./Assets/TypeScript/**/*.ts')
                        .pipe(ts({
                              sourcemap:true,
                              target:'ES3'
                        }))
                        .pipe(gulp.dest('./wwwroot/js'));
});

gulp.task('default',
  ['ts']);

```

这将在输出文件夹 wwwroot / js 中生成名为 inheritance.js.map 的源映射文件。它还将在生成的 inheritance.js 文件的底部添加以下行，以指示两者之间的关系： //# sourceMappingURL = inheritance.js.map 。

### EcmaScript 6

EcmaScript 6（ES6）是即将推出的新版 JavaScript。由于它是如此新颖，大多数浏览器还不支持它（或至少不完全支持）。这很可惜，因为你可以开始用它做的事情非常棒。幸运的是，对于开发人员来说，已经有一种方法可以利用它，然后将其转换为 EcmaScript，这是当前浏览器非常清楚的。一个例子是使用类和继承 - 我们知道它不是特别擅长的 JavaScript。参加以下 ES6 示例：