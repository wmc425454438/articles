# 使用 requestAnimationFrame

很长时间以来，计时器和定时执行都是 `JavaScript` 动画最先进的工具。自从。`Firefox 4` 的 `mozRequestAnimationFrame()` 出现之后，这个 API 被广泛采用，现在作为 `requestAnimationFrame()` 方法已经得到各大浏览器的支持。

## 1. 早期定时动画

以前，在 `JavaScript` 中创建动画基本上就是使用 `setInterval()` 来控制动画的执行。大概是下面的样子：

``` js
(function() {
    function updateAnimations() {
        doAnimation1();
        doAnimation2();
        // 其他任务
    }
    setInterval(updateAnimations, 100);
})();
```

一般计算机显示器的屏幕刷新率都是 `60Hz`，基本上意味着每秒需要重绘 `60` 次。因此，实现平滑动画最佳的重绘间隔为 `1000ms/60`，大约 `17` 毫秒。

## 2. 时间间隔的问题

`setInterval()` 和 `setTimeout()` 的不精确是个大问题，浏览器自身计时器的精度让这个问题雪上加霜。浏览器的计时器精度不足毫秒。以下是几个浏览器计时器的精度情况：

- `IE8` 及更早版本的计时器精度为 `15.625` 毫秒；
- `IE9` 及更晚版本的计时器精度为 `4` 毫秒；
- `Firefox` 和 `Safari` 的计时器精度为约 `10` 毫秒；
- `Chrome` 的计时器精度为 `4` 毫秒。

## 3. requestAnimationFrame

浏览器知道 `CSS` 过渡和动画应该什么时候开始，并据此计算出正确的时间间隔，到时间就去刷新用户界面。但对于 `JavaScript` 动画，浏览器不知道动画什么时候开始。所以需要创造一个方法，用以通知浏览器某些 `JavaScript` 代码要执行动画了。`requestAnimationFrame()` 方法接收一个参数，此参数是一个要在重绘屏幕前调用的函数。下面是一个例子：

``` html
<style>
  * {
    margin: 0 auto;
    padding: 0;
  }
  #status {
    display: inline-block;
    height: 100px;
    background-color: red;
    font-size: 30px;
    text-align: center;
    line-height: 100px;
    color: white;
  }
</style>

<!DOCTYPE html>
<html>
  <head>
    <title>ContextMenu Event Example</title>
  </head>
  <body>
    <div id="status" style="width: 1%"></div>
    <script>
      let last;
      function updateProgress(timestamp) { // timestamp 持续了多少ms
        let div = document.getElementById("status");
        div.style.width = parseInt(div.style.width, 10) + 1 + "%";
        div.innerHTML = div.style.width;
        if (last === undefined) {
          last = timestamp;
        }
        console.log(timestamp - last); // 每次改变的时间大约是16.6ms
        last = timestamp;
        if (div.style.width != "100%") {
          window.requestAnimationFrame(updateProgress);
        }
      }
      window.requestAnimationFrame(updateProgress);
    </script>
  </body>
</html>
```

因为 `requestAnimationFrame()` 只会调用一次传入的函数，所以每次更新用户界面时需要再手动调用它一次。同样，也需要控制动画何时停止。结果就会得到非常平滑的动画。

传给 `requestAnimationFrame()` 的函数实际上可以接收一个参数，此参数是一个 `DOMHighResTimeStamp` 的实例（比如 `performance.now()` 返回的值），表示下次重绘的时间。基于这个参数，就可以更好地决定如何调优动画了。

## 4. cancelAnimationFrame

与 `setTimeout()` 类似，`requestAnimationFrame()` 也返回一个请求 `ID`，可以用于通过另一个方法 `cancelAnimationFrame()` 来取消重绘任务。下面的例子展示了刚把一个任务加入队列又立即将其取消：

``` js
let requestID = window.requestAnimationFrame(() => {
    console.log('Repaint!');
});
window.cancelAnimationFrame(requestID);
```
