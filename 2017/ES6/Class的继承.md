# Class的继承

## 1.简介

Class 可以通过extends关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。
``` js
class Point {
}

class ColorPoint extends Point {
}
```

下面，我们在ColorPoint内部加上代码。

``` js
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```

上面代码中，constructor方法和toString方法之中，都出现了super关键字，它在这里表示父类的构造函数，用来新建父类的this对象。

子类必须在constructor方法中调用super方法，否则新建实例时会报错。
这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。

``` js
class Point { /* ... */ }

class ColorPoint extends Point {
  constructor() {
  }
}

let cp = new ColorPoint(); // ReferenceError
```

上面代码中，ColorPoint继承了父类Point，但是它的构造函数没有调用super方法，导致新建实例时报错。

最后，父类的静态方法，也会被子类继承。
``` js
class A {
  static hello() {
    console.log('hello world');
  }
}

class B extends A {
}

B.hello()  // hello world
```

上面代码中，hello()是A类的静态方法，B继承A，也继承了A的静态方法。


## 2.Object.getPrototypeOf() 
`Object.getPrototypeOf`方法可以用来从子类上获取父类。

``` js
Object.getPrototypeOf(ColorPoint) === Point
// true
```

**因此，可以使用这个方法判断，一个类是否继承了另一个类。**















