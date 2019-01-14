# 本地socket服务模拟--gulp

1. 首先需要在package.json中引入`nodejs-websocket`这个包，然后npm install一下。
``` json
{
  "name": "im",
  "version": "1.0.0",
  "main": ".server/web-socket-server.js",
  "dependencies": {
    "nodejs-websocket": "^1.7.1"
  },
  "devDependencies": {
    "gulp": "^3.9.1"
  }
}
```

2. 再来新建一个`web-socket-server`文件,用来模拟socket服务端的数据处理以及传输。
``` js
let ws = require("nodejs-websocket");

// 这个类是模拟webSocket服务端

module.exports = class WebSocketServer {
    constructor() {

    }

    create() {
        this.server = ws.createServer((conn) => {
            conn.on("text", (str) => {
                console.log("收到的信息为:" + str);
            });
            conn.on("close", function (code, reason) {
                console.log("关闭连接");
            });
            conn.on("error", function (code, reason) {
                console.log("异常关闭", code, reason)
            });
        }).listen(8001);
        this.server.on('connection', (conn) => {
            
        });
        console.log("WebSocket服务端建立完毕");
        return this;
    }

    close() {
        this.server.close();
        console.log("WebSocket服务关闭");
    }

    sendText(conn, msg, cbOk) {
        
    }
};
```

3. 最后再gulp中引入一下我们新建的文件`web-socket-server`
``` js
var gulp = require("gulp");
var WebSocketServer = require("./.server/web-socket-server");
var socketServer = new WebSocketServer();
gulp.task("default", function () {
    return socketServer.create();
});

gulp.task("travis", ["default"], function (res) {
    setTimeout(function () {
        socketServer.close();
        socketServer = null;
    }, 4000);
});
```

4. 运行一下gulp搞定
