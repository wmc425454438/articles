## 宏观任务和微观任务
- 宿主环境发起的任务叫做宏观任务。
- javascript引擎发起的任务叫做微观任务。
- 微观任务始终先于宏观任务执行。promise是微观任务，setTimeout是宏观任务。

例题：我们现在要实现一个红绿灯，把一个圆形 div 按照绿色 3 秒，黄色 1 秒，红色 2 秒循环改变背景色，你会怎样编写这个代码呢？

用promise把setTimeout函数封装成为可用于异步的函数

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    .trafic-light {
      height: 100px;
      width: 100px;
      border-radius: 50%;
      margin: 200px auto 0;
      border: 1px solid #ececec;
    }
  </style>
</head>
<body>
  <div id="trafic-light" class="trafic-light"></div>
</body>
<script>
  let traficEle = document.getElementById('trafic-light');
  function changeTraficLight(color, duration) {
    return new Promise(function (resolve, reject){
      traficEle.style.background = color;
      setTimeout(resolve, duration);
    })
  }
  async function traficScheduler() {
    await changeTraficLight('green', 3000)
    await changeTraficLight('yellow', 1000)
    await changeTraficLight('red', 2000)
    traficScheduler();
  }
  traficScheduler();
</script>
</html>
```
