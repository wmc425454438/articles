# sonarlint-Vscode本地配置方案

## 前言

为了保证我们的代码质量，公司让我们的代码都需要经过`sonarlint`的静态代码检查才能发布到生产环境。但我们现在所用的前端代码大部分都是`eslint`来检查代码，规则和`sonarlint`不太一样。所以有些报错在本地不会提示，但是到了`sonarQube`上就会变成严重的错误警告，而不能发布生产版本。为了可以在本地一次性将代码通过`sonarlint`的检测，节约时间成本，对如何在本地使用`sonarlint`进行了研究。

## 工作

在vscode配置`sonarlint`我们需要做如下工作：

1. 下载`sonarlint`的vscode的插件，并安装。
2. 登录自己的`sonarQube`账号，获得自己的`generate token`。
3. 配置自己的`vscode`的`settings.json`

## 下载并安装sonarlint插件

一般在扩展里面直接搜索`sonarlint`直接安装。或者官网下载[sonarlint插件](https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarlint-vscode)。通过安装插件包的方式安装。

这是下载插件的位置
![image](%E4%B8%8B%E8%BD%BD%E6%89%A9%E5%B1%95%E4%BD%8D%E7%BD%AE.png)
这是安装插件的方式
![image1](vscode%E6%89%A9%E5%B1%95%E5%AE%89%E8%A3%85.png)

## 获取Generate Token

先登录自己的`sonarQube` => 点击自己的头像 => 进入myAccount => 进入Security一栏 => 写一个自己定义的token name => 复制生成的token并保存好。
注意这个token只能生成时看到，后面就看不到了，所以要保存好，配置时要用。

![token](sonarlint-token%E8%8E%B7%E5%8F%96.png)

## 配置settings.json

找到自己已经安装好的`sonarlint`插件，进入介绍部分。可以看到介绍部分有教我们如何配置自己的`sonarlint`。下面是我自己的配置，可以参照我的配置进行修改。

``` json
"sonarlint.connectedMode.project": {
        "serverId": "warming", // 自定义的id要和下面的serverId保持一致
        "projectKey": "juejin:frontend:master", // 在sonarQube可以看到项目的key
},
"sonarlint.connectedMode.servers": [
    {
        "serverId": "warming", // 和上面的serverId相同
        "serverUrl": "https://sonar.juejin.im/", // 这个不用改动，大家的url相同
        "token": "******************" // 之前获取到自己的token
    }
],
"sonarlint.ls.javaHome": "C:\\\\Program Files\\Java\\jdk1.8.0_91", // 需要配置一下javaHome
```

## 结束

至此配置结束，重启`vscode`可以看到`sonarlint`的错误提示。现在可以有效避免影像生产严重警告的问题了。

## sonarlint

在某些方面`sonarlint`做得还是不错的，很多代码逻辑上的错误也会提示。而`eslint`注重的是语法上的错误。

类似下面的代码块，`sonarlint`会提示不安全的写法，而`eslint`则不会。理由是`finally`如果写了`return`会覆盖掉`try`和`catch`中的`return`，任何情况下都会返回`finally`下的`return`，不符合我们的常规逻辑，所以会报错。

``` js
function final() {
    try{
        // doSomething();
        return true;
    } catch(err) {
        return false;
    } finally {
        return 'is over';
    }
}
```
