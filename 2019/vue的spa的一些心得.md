## Router-view

SPA单页应用，通过router来控制页面的转换。每个页面其实就是一个组件，通过在router的index.js中引用，可以达到单页应用的效果。

## 过渡

`<transition />`组件可以帮助我们写组件的过渡效果。因为router-view也是一个组件，所以同样可以用来渲染页面跳转的过渡效果。

## 全屏监听

- F11 key can exit programmatic-fullscreen, but programmatic-exitFullscreen cannot exit F11-fullscreen. Which is the problem you are running into. Also, Esc key cannot exit F11-fullscreen, but does exit programmatic-fullscreen.
- F11可以退出程序控制的全屏，但是程序控制的退出全屏不能退出F11的全屏。并且，Esc键不能退出F11的全屏，但可以退出Esc的全屏。

``` js
 document.onfullscreenchange = (event) => {
      console.log('onfullscreenchange', event)
      this.fullscreen = document.fullscreenElement !== null
 }
```

貌似可以引入fullscreen一个npm包，然后用他解决全屏的问题，省的自己动手写了。

## axios
可以现在api中将axios请求封装好之后，再在其他页面进行引用。其中有一个拦截请求的用法，可以在发送请求前进行拦截，并添加一些数据。与之相对的是在发送请求之后可以添加一些参数用于返回调用。

## flex
在使用flex的时候发现，用百分比比具体宽度会好很多，不用担心两边的距离不一样。
一行多列的时候使用百分比，padding值来提供间隔空间，注意盒模型要更换为border-box。整体宽度要加上padding值。

## IE兼容
IE不兼容ES6的简写语法，只能将其转换为ES5语法。进场会报错`SCRIPT:1003 缺少:`。
``` js
var vue = new Vue({
	data(){
    return {
      params,
      }
    }
})
```

改写后
``` js
var vue = new Vue({
  data: function data() {
    return {
      params: params
    };
  }
});
```
