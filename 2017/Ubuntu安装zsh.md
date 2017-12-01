# 安装ZSH

>ZSH是美化终端的一个插件，在用终端的时候可以显得更酷一点把。


先安装一下
``` bash
$ sudo apt-get install zsh
```

然后替换默认shell
``` bash
$ chsh -s /bin/zsh
```

安装curl
``` bash
$ apt-get install curl
```

安装oh-my-zsh
``` bash
$ curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

在根目录下面打开~/.zshrc配置文件，最好安装下vim，还没装的用vi也可以
``` bash
$ vim ~/.zshrc

#or

$ vi ~/.zshrc
```

更改ZSH主题配置
``` bash
ZSH-THEME="agnoster"
```

安装powerline字体
``` bash
$ git clone https://github.com/powerline/fonts.git

$ cd fonts
$ ./install.sh

$ cd ..
$ rm -rf fonts
```

### 最后设置一下终端的字体
先打开终端的`配置`窗口,找到`配置文件`，编辑配置文件（如果你是第一次配置，那一般都是默认的`Undefined`），设置一下`常规`中的`自定义字体`，找到`powerline`中随便设置一个就可以了。
