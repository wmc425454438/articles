# DOM扩展

- 理解 Selectors API
- 使用 HTML5 DOM 扩展

## Selectors API

`JavaScript` 库中最流行的一种能力就是根据 `CSS` 选择符的模式匹配 `DOM` 元素。`Selectors API` 是 `W3C` 推荐标准，规定了浏览器原生支持的 `CSS` 查询 `API`。

支持这一特性的所有 `JavaScript` 库都会实现一个基本的 `CSS` 解析器，然后使用已有的 `DOM` 方法搜索文档并匹配目标节点。

看一下 [`W3C`](https://www.w3.org/TR/selectors-api2 "https://www.w3.org/TR/selectors-api2") 定义的 `Selectors API Level 2`：

``` java
partial interface Document {
  Element?  querySelector(DOMString selectors);
  NodeList  querySelectorAll(DOMString selectors);

  Element?  find(DOMString selectors, optional (Element or sequence<Node>)? refNodes);
  NodeList  findAll(DOMString selectors, optional (Element or sequence<Node>)? refNodes);
};

partial interface DocumentFragment {
  Element?  querySelector(DOMString selectors);
  NodeList  querySelectorAll(DOMString selectors);

  Element?  find(DOMString selectors, optional (Element or sequence<Node>)? refNodes);
  NodeList  findAll(DOMString selectors, optional (Element or sequence<Node>)? refNodes);
};

partial interface Element {
  Element?  querySelector(DOMString selectors);
  NodeList  querySelectorAll(DOMString selectors);

  Element?  find(DOMString selectors);
  NodeList  findAll(DOMString selectors);

  boolean   matches(DOMString selectors, optional (Element or sequence<Node>)? refNodes);
};
```

`Selectors API Level 1` 的核心是两个方法：`querySelector()`和 `querySelectorAll()`。在兼容浏览器中，`Document` 类型和 `Element` 类型的实例上都会暴露这两个方法。

`Selectors API Level 2` 规范在 `Element` 类型上新增了更多方法，比如 `matches()` 、 `find()` 和 `findAll()` 。不过，目前还没有浏览器实现或宣称实现 `find()`和 `findAll()`。

### 1. `querySelector()`

`querySelector()`方法接收 `CSS` 选择符参数，返回匹配该模式的第一个后代元素，如果没有匹配项则返回 `null`。下面是一些例子：

``` js
// 取得<body>元素
let body = document.querySelector("body");
// 取得 ID 为"myDiv"的元素
let myDiv = document.querySelector("#myDiv");
// 取得类名为"selected"的第一个元素
let selected = document.querySelector(".selected");
// 取得类名为"button"的图片
let img = document.body.querySelector("img.button");
```

在 `Document` 上使用 `querySelector()`方法时，会从文档元素开始搜索；在 `Element` 上使用`querySelector()`方法时，则只会从当前元素的`后代`中查询。

用于查询模式的 `CSS` 选择符可繁可简，依需求而定。如果选择符有语法错误或碰到不支持的选择符，则 `querySelector()`方法会抛出错误。

### 2. `querySelectorAll()`

`querySelectorAll()`方法跟 `querySelector()`一样，也接收一个用于查询的参数，但它会返回所有匹配的节点，而不止一个。这个方法返回的是一个 `NodeList` 的静态实例。

与 `querySelector()`一样，`querySelectorAll()`也可以在 `Document`、`DocumentFragment` 和 `Element` 类型上使用。下面是几个例子：

``` js
// 取得 ID 为"myDiv"的<div>元素中的所有<em>元素
let ems = document.getElementById("myDiv").querySelectorAll("em");
// 取得所有类名中包含"selected"的元素
let selecteds = document.querySelectorAll(".selected");
// 取得所有是<p>元素子元素的<strong>元素
let strongs = document.querySelectorAll("p strong");
```

返回的 `NodeList` 对象可以通过 `for-of` 循环、`item()`方法或中括号语法取得个别元素。比如：

``` js
let strongElements = document.querySelectorAll("p strong");
// 以下 3 个循环的效果一样
for (let strong of strongElements) {
 strong.className = "important";
}
for (let i = 0; i < strongElements.length; ++i) {
 strongElements.item(i).className = "important";
}
for (let i = 0; i < strongElements.length; ++i) {
 strongElements[i].className = "important";
} 
```

与 `querySelector()`方法一样，如果选择符有语法错误或碰到不支持的选择符，则 `querySelectorAll()`方法会抛出错误。

如果需要 `querySelectorAll()` 只是搜索 element 的后代中的元素，我们需要用到 `:scope` 伪类来符合预期。下面是一个例子

``` html
<div class="outer">
  <div class="select">
    <div class="inner">
    </div>
  </div>
</div>
```

``` js
var select = document.querySelector('.select');
var inner = select.querySelectorAll('.outer .inner');
var scope_inner = select.querySelectorAll(':scope .outer .inner');
inner.length; // 1
scope_inner.length; // 0
```

### 3. `matches()`

`matches()`方法接收一个 `CSS` 选择符参数，如果元素匹配则该选择符返回 `true`，否则返回 `false`。例如：

``` js
if (document.body.matches("body.page1")){
 // true
}
```

使用这个方法可以方便地检测某个元素会不会被 `querySelector()`或 `querySelectorAll()`方法返回。所有主流浏览器都支持 `matches()`。`Edge`、`Chrome`、`Firefox`、`Safari` 和 `Opera` 完全支持，`IE9~11` 及一些移动浏览器支持带前缀的方法。

## HTML5

> `HTML5` 规范包含了与标记相关的大量 `JavaScript API` 定义。其中有的 `API` 与 `DOM` 重合，定义了浏览器应该提供的 `DOM` 扩展。

### 1. CSS 类扩展

自 `HTML4` 被广泛采用以来，`Web` 开发中一个主要的变化是 `class` 属性用得越来越多，其用处是为元素添加样式以及语义信息。为了适应开发者和他们对 `class` 属性的认可，`HTML5` 增加了一些特性以方便使用 `CSS` 类。


#### 1. getElementsByClassName()

`getElementsByClassName()` 是 `HTML5` 新增的最受欢迎的一个方法，暴露在 `document` 对象和所有 `HTML` 元素上。这个方法脱胎于基于原有 `DOM` 特性实现该功能的 `JavaScript` 库，提供了性能高好的原生实现。

`getElementsByClassName()` 方法接收一个参数，即包含一个或多个类名的字符串，返回类名中包含相应类的元素的 `NodeList`。如果提供了多个类名，则顺序无关紧要。下面是几个示例：

``` js
// 取得所有类名中包含"username"和"current"元素
// 这两个类名的顺序无关紧要
let allCurrentUsernames = document.getElementsByClassName("username current");
// 取得 ID 为"myDiv"的元素子树中所有包含"selected"类的元素
let selected = document.getElementById("myDiv").getElementsByClassName("selected");
```

如果要给包含特定类（而不是特定 `ID` 或标签）的元素添加事件处理程序，使用这个方法会很方便。不过要记住，因为返回值是 `NodeList`，所以使用这个方法会遇到跟使用 `getElementsByTagName()` 和其他返回 `NodeList` 对象的 `DOM` 方法同样的问题。

#### 2. classList 属性

要操作类名，可以通过 `className` 属性实现添加、删除和替换。但 `className` 是一个字符串，所以每次操作之后都需要重新设置这个值才能生效，即使只改动了部分字符串也一样。以下面的 `HTML` 代码为例：

``` html
<div class="bd user disabled">...</div>
```

这个 `<div>` 元素有 `3` 个类名。要想删除其中一个，就得先把 `className` 拆开，删除不想要的那个，再把包含剩余类的字符串设置回去。比如：

``` js
// 要删除"user"类
let targetClass = "user";
// 把类名拆成数组
let classNames = div.className.split(/\s+/);
// 找到要删除类名的索引
let idx = classNames.indexOf(targetClass);
// 如果有则删除
if (idx > -1) {
 classNames.splice(i,1);
}
// 重新设置类名
div.className = classNames.join(" ");
```

`HTML5` 通过给所有元素增加 `classList` 属性为这些操作提供了更简单也更安全的实现方式。`classList` 是一个新的集合类型 `DOMTokenList` 的实例。与其他 DOM 集合类型一样，`DOMTokenList` 也有 `length` 属性表示自己包含多少项，也可以通过 `item()` 或中括号取得个别的元素。此外，`DOMTokenList` 还增加了以下方法。

- `add(value)`，向类名列表中添加指定的字符串值 `value`。如果这个值已经存在，则什么也不做。
- `contains(value)`，返回布尔值，表示给定的 `value` 是否存在。
- `remove(value)`，从类名列表中删除指定的字符串值 `value`。
- `toggle(value)`，如果类名列表中已经存在指定的 `value`，则删除；如果不存在，则添加。
- `replace(old, new)`，如果类名列表中已经存在指定的 `old`，则替换成 `new`；如果不存在，则什么也不做。

这样以来，前面的例子中那么多行代码就可以简化成下面的一行：

``` js
div.classList.remove("user"); 
```

这行代码可以在不影响其他类名的情况下完成删除。其他方法同样极大地简化了操作类名的复杂性，如下面的例子所示：

``` js
// 删除"disabled"类
div.classList.remove("disabled");
// 添加"current"类
div.classList.add("current");
// 切换"user"类
div.classList.toggle("user");
// 检测类名
if (div.classList.contains("bd") && !div.classList.contains("disabled")){
 // 执行操作
)
// 迭代类名
for (let class of div.classList){
 doStuff(class);
}
// 将类值 "foo" 替换成 "bar"
div.classList.replace("foo", "bar");
```

添加了 `classList` 属性之后，除非是完全删除或完全重写元素的 `class` 属性，否则 `className`属性就用不到了。