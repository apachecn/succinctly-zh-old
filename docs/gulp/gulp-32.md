  

代码 28：inheritance.ts - /Assets/TypeScript/inheritance.ts

```
class
  Animal {

  constructor(public name: string) { }

  move(meters: number) {

  alert(this.name + " moved " + meters + "m.");

  }
}

class
  Snake extends Animal {

  constructor(name: string) { super(name); }

  move() {

  alert("Slithering...");

  super.move(5);

  }
}

class
  Horse extends Animal {

  constructor(name: string) { super(name); }

  move() {

  alert("Galloping...");

  super.move(45);

  }
}

var
  sam = new Snake("Sammy the Python");
var
  tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);

```

有几个插件可用于将 TypeScript 编译为 JavaScript。我选择 gulp-typescript-compiler ，但你可以尝试其他任何一个。

代码 29：用于编译 TypeScript 的 gulpfile.js - /gulpfile.js

```
var gulp = require('gulp'),
      ts
  = require('gulp-typescript-compiler');

gulp.task('ts', function () {

  return gulp.src('./Assets/TypeScript/**/*.ts')
                        .pipe(ts())
                        .pipe(gulp.dest('./wwwroot/js'));
});

gulp.task('default', ['ts']);

```

这导致以下 EcmaScript 5 代码：