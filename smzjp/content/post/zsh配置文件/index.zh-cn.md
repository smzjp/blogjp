---
original: true
title: zsh配置文件
description: .zprofile和.zshrc的区别
date: 2023-01-15
slug: 
image: 
tags:
    - zsh
categories:
    - 
---

macOS 从Monterey开始改变了默认的shell解释器，之前是bash，现在改成了zsh。

### 命令

1. 查看当前系统所有安装的shell解释器。 `cat /etc/shells`
2. 查看当前使用的是哪个shell解释器 `echo $SHELL`
3. 修改使用的shell。`chsh -s /bin/bash`。把shell解释器改为bash。

### zsh

在用户根目录下可以看到两个隐藏的 .zprofile和.zshrc 文件。.zprofile只在电脑开机的时候加载一次，适合用来存放环境变量。.zshrc是每次启动shell终端时会加载。.zshrc的配置会覆盖.zprofile里面的配置如果出现一样的。



**参考文档:**[https://www.zerotohero.dev/zshell-startup-files/](https://www.zerotohero.dev/zshell-startup-files/)

### 环境变量设置

#### 临时设置

在当前的终端执行

```
export PATH=`pwd`/flutter/bin:$PATH
```

只会在当前的终端生效，如果当前终端关闭，下次打开要重新设置

#### 永久设置

1. 在用户根目录（～）下 找到.zprofile文件，注意是隐藏文件，可以通过`ls`命令显示，通过`vim .zprofile`打开，然后把下面代码添加到 .zprofile 里。

   ```
   export PATH=FLUTTER_INSTALL_PATH/flutter/bin:$PATH
   
   //比如本机的flutter安装路径是~/development/
   export PATH=~/development/flutter/bin:$PATH
   ```

   FLUTTER_INSTALL_PATH是软件安装路径，这里是以flutter举例，其他软件也是一样。路径可以通过`which flutter`查看。

2. 运行 `source $HOME/.zprofile` 刷新当前终端窗口。 

3. 验证添加全局PATH是否成功`echo $PATH`。添加成功后，你就可以在任何打开的终端执行flutter的命令了，再次执行`flutter doctor`就不会报*zsh: command not found: flutter*的错误了。









