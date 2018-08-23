# windows下用nvm安装node和npm

今天在windows下使用nvm下载了node和npm，踩了一点坑，记录一下～以备以后使用～

## 1、安装nvm

+ 下载链接：

  [Github： Download nvm-windows --- nvm-setup.zip](https://github.com/coreybutler/nvm-windows/releases)

+ 有一个要注意的点是：

  >  It comes with an installer (and uninstaller), because getting it should be easy. Please note, you need to uninstall any existing versions of node.js before installing NVM for Windows. Also delete any existing nodejs installation directories (e.g., "C:\Program Files\nodejs") that might remain. NVM's generated symlink will not overwrite an existing (even empty) installation directory.  
  >
  > ————nvm的readme

  意思就是如果你电脑上已经有已经存在的node.js一定要先清除掉，不过由于我本来电脑上就没有，所以就没有关系啦～

+ 安装完以后，打开命令行，输入`nvm v`如果出来它的版本号就说明已经安装成功～



## 2、安装node和npm

+ 首先，我们要给node添加个镜像～

  + 在nvm的安装路径下，找到setting.txt，加上下面这行：

    ```
    node_mirror: https://npm.taobao.org/mirrors/node/
    ```

    nvm的安装路径可能存在隐藏文件，可以通过设置显示；

+ 在命令行输入`nvm install latest`安装最新稳定版本的node；

+ 过了一会儿，你会发现，node和npm都装好了！这个时候**敲重点！**

  **不要`use node 版本号`**，如果这么做的话npm会报错；

  楼主执行这一步以后，想查看一下npm的版本，结果得到这个错误。。

  ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1fs0p5h9etrj20hq0e3wf9.jpg)

  

  **那要怎么办呢？？**

  + 在node官网下载同版本的node.js的*.zip：

    [https://nodejs.org/en/download/current/](https://nodejs.org/en/download/current/)

  + 替换nvm安装路径下的该node版本的npm文件；

  + 然后再`nvm use node版本号`～

  + 成功～～

+ 最最后，安装一下npm的设置：

  `npm config set registry http://registry.npm.taobao.org/`



## The last

参考链接：

nvm的windows下载：<https://segmentfault.com/a/1190000007612011> 

node镜像的安装：<https://github.com/xhlwill/blog/issues/7> 

npm报错：https://github.com/coreybutler/nvm-windows/issues/342

撒花～～