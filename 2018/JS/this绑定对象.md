# this绑定对象

学习 this 的第一步是明白 this 既不指向函数自身也不指向函数的词法作用域，你也许被这样的解释误导过，但其实它们都是错误的。

this 实际上是在函数被调用时发生的绑定，它指向什么完全取决于函数在哪里被调用。

``` js
function foo() {
	var a = 2;
	this.bar();
}
function bar() {
	console.log( this.a );
}
foo(); // ReferenceError: a is not defined
```
这里的` this `其实指向的是全局对象` a `，因为` bar() `是全局环境下运行的函数。执行的是RHS。


调用位置和调用栈可以帮我们清楚的分析` this `的调用位置。
``` js
function baz() {
// 当前调用栈是：baz
// 因此，当前调用位置是全局作用域
console.log( "baz" );
bar(); // <-- bar 的调用位置
}
function bar() {
// 当前调用栈是 baz -> bar
// 因此，当前调用位置在 baz 中
console.log( "bar" );
foo(); // <-- foo 的调用位置
}
function foo() {
// 当前调用栈是 baz -> bar -> foo
// 因此，当前调用位置在 bar 中
console.log( "foo" );
}
baz(); // <-- baz 的调用位置
```
- 隐式绑定
``` js
function foo() {
console.log( this.a );
}
var obj2 = {
a: 42,
foo: foo
};
var obj1 = {
a: 2,
obj2: obj2
};
obj1.obj2.foo(); // 42
```
只会绑定最后的对象。
- 显式绑定
` call() `和` apply() `方法
``` js
function foo() {
console.log( this.a );
}
var obj = {
a:2
};
foo.call( obj ); // 2

// 硬绑定
function foo() {
console.log( this.a );
}
var obj = {
a:2
};
var bar = function() {
foo.call( obj );
};
bar(); // 2
setTimeout( bar, 100 ); // 2
// 硬绑定的 bar 不可能再修改它的 this
bar.call( window ); // 2
```

由于硬绑定是一种非常常用的模式，所以在 ES5 中提供了内置的方法` Function.prototype.bind `，它的用法如下：
``` js
function foo(something) {
console.log( this.a, something );
return this.a + something;
}
var obj = {
a:2
};
var bar = foo.bind( obj );
var b = bar( 3 ); // 2 3
console.log( b ); // 5
```

- 间接绑定
``` js
// 间接绑定容易在赋值时发生，会应用默认绑定
function foo() {
  console.log( this.a );
}
var a = 2;
var o = { a: 3, foo: foo };
var p = { a: 4 };
o.foo(); // 3
(p.foo = o.foo)(); // 2
```
如果函数体处于严格模式，` this `会被绑定到` undefined `，否则` this `会被绑定到全局对象。

- 软绑定
``` js
if (!Function.prototype.softBind) {
	Function.prototype.softBind = function(obj) {
		var fn = this;
// 捕获所有 curried 参数
var curried = [].slice.call( arguments, 1 );
var bound = function() {
	return fn.apply(
		(!this || this === (window || global)) ?
		obj : this
		curried.concat.apply( curried, arguments )
		);
};
bound.prototype = Object.create( fn.prototype );
return bound;
};
}
```
-----------------
箭头函数最常用于回调函数中，例如事件处理器或者定时器：

``` js
function foo() {
  setTimeout(() => {
    // 这里的 this 在此法上继承自 foo()
      console.log( this.a );
    },100);
}
var obj = {
  a:2
};
foo.call( obj ); // 2
```

箭头函数可以像 bind(..) 一样确保函数的 this 被绑定到指定对象，此外，其重要性还体现在它用更常见的词法作用域取代了传统的 this 机制。
---------------
如果要判断一个运行中函数的` this `绑定，就需要找到这个函数的直接调用位置。找到之后就可以顺序应用下面这四条规则来判断` this `的绑定对象。

1. 由` new `调用？绑定到新创建的对象。
2. 由` call `或者` apply`（或者` bind `）调用？绑定到指定的对象。
3. 由上下文对象调用？绑定到那个上下文对象。
4. 默认：在严格模式下绑定到` undefined`，否则绑定到全局对象。


一定要注意，有些调用可能在无意中使用默认绑定规则。如果想“更安全”地忽略 this 绑定，你可以使用一个 DMZ 对象，
比如 ø = Object.create(null)，以保护全局对象。

ES6 中的`箭头函数`并不会使用四条标准的绑定规则，而是根据当前的词法作用域来决定this，
具体来说，箭头函数会继承外层函数调用的 this 绑定（无论 this 绑定到什么）。这
其实和 ES6 之前代码中的 self = this 机制一样。
