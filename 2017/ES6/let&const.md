# let & const

## 块级作用域`let`

`let`的出现很好的解决了立即执行函数表达式(IIFE)带来的烦恼

``` js
//before
(function(){
  var temp = ...;
  //...
})();

//now
{
  let temp = ...;
  //...
}
```

### for

`let`还很好的解决了for循环之前出现变量泄露的问题
``` js
//before
for(var i = 0;i < 5; i++){
  //...
}
console.log(i);// 5

//now
for(let i = 0;i < 5; i++){
  //...
}
console.log(i);// RefrenceError
```
for循环结束后，我们并不需要`i`这个变量了，但是却变成了全局变量.

用`let`声明的变量在循环结束后销毁，只在for的循环中起到作用。

这里还有一件有意思的事：

`for`循环中用到的`let`定义的`i`，是一个循环内容的父作用域，而`for`循环内容是子作用域。

``` js
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```

## `const`定义常量

`const`声明一个只读的常量。一旦声明，值就不能改变。
``` js
const PI = 3.14;
PI = 3; // Assignment to constant variable.
```
和`let`一样都是只在声明的块级作用域有效。

``` js
if(true){
  const a = 1;
  //...
}
console.log(a);// RefrenceError
```

## ES6 声明变量的方法
一共有6种：
1. let
2. const
3. var
4. function
5. import
6. class
