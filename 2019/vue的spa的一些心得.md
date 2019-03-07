## Router-view

SPA单页应用，通过router来控制页面的转换。每个页面其实就是一个组件，通过在router的index.js中引用，可以达到单页应用的效果。

## 过渡

`<transition />`组件可以帮助我们写组件的过渡效果。因为router-view也是一个组件，所以同样可以用来渲染页面跳转的过渡效果。

## 全屏监听

``` js
 document.onfullscreenchange = (event) => {
      console.log('onfullscreenchange', event)
      this.fullscreen = document.fullscreenElement !== null
 }
```
