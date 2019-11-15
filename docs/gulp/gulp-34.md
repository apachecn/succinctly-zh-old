  

代码 32：ES6 文件 inheritance.js - /Assets/ES6/inheritance.js

```
class Shape {

  constructor(id, x, y) {

  this.id = id

  this.move(x, y)

  }

  move(x, y) {

  this.x = x

  this.y = y

  }
}

class Rectangle extends Shape {

  constructor(id, x, y, width, height) {

  super(id, x, y)

  this.width = width

  this.height = height

  }
}

class Circle extends Shape {

  constructor(id, x, y, radius) {

  super(id, x, y)

  this.radius = radius

  }
}

var c = new
  Circle('firstCircle', 3, 4, 5);
console.log(c);
c.move(10,
  20);
console.log(c);

```

它有一个类 Shape ，它具有移动功能。另外两个类， Circle 和 Rectangle ，继承自它。在类声明之后，还有四行，它们实例化一个新的 Circle 对象并将一些参数传递给它的构造函数，该构造函数又调用基类构造函数以及 super（）调用。要查看对象本身，我们将其写入控制台。然后我们将圆形对象移动到一些新的 x：y 坐标并再次将其写入控制台。嘿，这可能是一个有趣的新游戏的开始！

然而，要将其转换为 ES5，我们需要一些帮助。在编写 Gulp 插件的时候，一旦 ES6 起飞，很快就会有更多。我们的 Gulp 文件如下所示：