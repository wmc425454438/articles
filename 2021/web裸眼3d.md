---
theme: jzman
highlight: zenburn
---

**这是我参与8月更文挑战的第5天，活动详情查看：[8月更文挑战](https://juejin.cn/post/6987962113788493831 "https://juejin.cn/post/6987962113788493831")**

## 前言

看了首页推荐裸眼3D的实现，觉得很酷炫。仔细一看是 `android` 端去实现的，我这个普通前端看的不是很懂，但至少思路和大致的方法学到了。正好前两天在学习 `web` 浏览器事件，看到了 `web` 的`陀螺仪`事件，正好和文中提到的`陀螺仪`一致。想着是不是也可以实现 `web` 端的裸眼3D呢？

在查阅MDN之后，知道移动端浏览器基本都支持了 `deviceorientation` -- 陀螺仪事件。下面就开始了我的研究。

## deviceorientation

`deviceorientation` 是`DeviceOrientationEvent`规范定义的事件，这个事件就会在 `window` 上触发。当 `deviceorientation` 触发时，`event` 对象中会包含各个轴相对于设备静置时坐标值的变化，主要是以下 `5` 个属性。

- alpha：`0~360` 范围内的浮点值，表示围绕 `z` 轴旋转时 `y` 轴的度数（左右转）。
- beta：`–180~180` 范围内的浮点值，表示围绕 `x` 轴旋转时 `z` 轴的度数（前后转）。
- gamma：`–90~90` 范围内的浮点值，表示围绕 `y` 轴旋转时 `z` 轴的度数（扭转）。
- absolute：布尔值，表示设备是否返回绝对值。
- compassCalibrated：布尔值，表示设备的指南针是否正确校准。

这里我只用到了 `beta` 和 `gamma`，水平上的旋转 `alpha` 没有用到。

### 监听

因为 `MDN` 上标注了 `chrome` 浏览器支持 `deviceorientation` 事件。所以就按照下面的方式写了一个监听事件：

``` js
window.addEventListener("deviceorientation", function(event) {
    console.log(event.beta, event.gamma);
});
```

天真的以为可以获取到事件的值，但是调试半天都没有获取到。
后来查询 `ios` 获取 `DeviceOrientationEvent` 事件，需要两个条件：

- 用户手动点击 `button` 按钮获取 `requestPermission` 权限
- 网络协议必须为 `https`

按钮权限问题可以通过添加代码解决，代码如下：

``` js
$('.btn').on('click', function() {
    if (typeof DeviceOrientationEvent.requestPermission === 'function') {
        DeviceOrientationEvent.requestPermission()
            .then(permissionState => {
                if (permissionState === 'granted') {
                // handle data
                window.addEventListener("deviceorientation", function(event) {
                    console.log(event.beta, event.gamma);
                });
                } else {
                // handle denied
                }
            })
            .catch((err) => {
                console.log(err)
            });
    }
});
```

因为我是用 `vscode live sever` 去启动的本地 `html` 所以需要去 `live sever` 扩展中配置 `https` 的相关信息。首先生成 `https` 需要的相关文件，最后配置一下就可以了。

``` json
"liveServer.settings.https": {
    "enable": true, //set it true to enable the feature.
    "cert": "D:\\https\\example.com+5.pem", //full path
    "key": "D:\\https\\example.com+5-key.pem", //full path
    "passphrase": ""
}
```

这两项都弄好之后，陀螺仪监听事件也如期运行了。

## 图层

按照参考的两篇文章都是3层图层：

- 上层负责跟踪手机偏移方向
- 中层不动
- 下层与手机偏移方向相反移动

实现图层用 `z-index` 就可以，其余就是简单的设置背景图，大致代码如下：

``` css
.container {
    position: relative;
    width: 100%;
    height: 100%;
}
.top {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: url(./img/top.png);
    background-size: cover;
    z-index: 300;
}
.middle {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: url(./img/middle.png);
    background-size: cover;
    z-index: 200;
}
.bottom {
    position: absolute;
    top: -30;
    left: -30;
    right: -30;
    bottom: -30;
    background: url(./img/bottom.png);
    background-size: cover;
    z-index: 100;
}
```

要实现偏移最简单的方式就是更改 `top` 和 `left` 这两个属性值了：

``` js
window.addEventListener("deviceorientation", function(event) {
    document.querySelector('.top').style.top = 0 + event.beta + 'px';
    document.querySelector('.top').style.left = 0 + event.gamma + 'px';
    document.querySelector('.bottom').style.top = -30 - event.beta + 'px';
    document.querySelector('.bottom').style.left = -30 - event.gamma + 'px';
});
```

## 引用

- [自如客APP裸眼3D效果的实现](https://juejin.cn/post/6989227733410644005#comment "https://juejin.cn/post/6989227733410644005#comment")

- [拿去吧你！Flutter 仿自如 App 裸眼 3D 效果｜ 8月更文挑战](https://juejin.cn/post/6991409083765129229?from=main_page "https://juejin.cn/post/6991409083765129229?from=main_page")