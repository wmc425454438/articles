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
