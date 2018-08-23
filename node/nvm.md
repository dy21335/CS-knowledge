## 由react引发的一场血案

**想装个开发react的环境，本机上之前已经安装了node，直接npm发现各种问题，什么权限啦，更新啦，真是太抓瞎了。。**

原因在于，我之前是直接从node官网下载的。

> 在官网下载的 node 安装包，运行后会自动安装在全局目录，使用过程中经常会遇到一些**权限问题**。

> Node.js安装过程将npm安装在仅具有本地权限的目录中。 当您尝试全局运行包时，这可能会导致权限错误。(npm官网翻译)

**怎么解决这个问题呢？**

> 以下来自npm官网：
>
> 为了解决这两个问题，许多开发人员选择使用节点版本管理器（nvm）来安装npm。 版本管理器将避免权限错误，并且将解决更新Node.js和npm的复杂性。
>
> 另外，开发人员可以使用nvm在npm的多个版本上测试他们的应用程序。 nvm使您可以轻松切换npm以及节点版本。



## 先清除余孽

由上可知，我们需要安装nvm，在安装之前，我们最好把之前已经安装的node.js清除掉。

>首先，打开你 Finder，按 `shift+command+G`，打开前往文件夹的窗口，分别输入下列目录进去之后删除 `node` 和 `node_modules` 相关的文件和文件夹:
>
>- 打开 `/usr/local/lib`，删除 `node` 和 `node_modules` 相关的文件和文件夹;
>- 打开 `/usr/local/include`，删除 `node` 和 `node_modules` 相关的文件和文件夹;
>- 如果你是使用的 `brew install node` 安装的 NodeJS，那么你还需要在终端中执行 `brew uninstall node` 命令来卸载;
>- 检查你的个人主文件夹下面的所有的 `local`、`lib` 以及 `include` 文件夹，并且删除所有与 `node` 和 `node_modules` 相关的文件以及文件夹;
>- 打开 `/usr/local/bin` 并删除 `node` 可执行文件。

此外，在终端进行一些额外操作：

```shell
sudo rm /usr/local/bin/npm
sudo rm /usr/local/share/man/man1/node.1
sudo rm /usr/local/lib/dtrace/node.d
sudo rm -rf ~/.npm
sudo rm -rf ~/.node-gyp
sudo rm /opt/local/bin/node
sudo rm /opt/local/include/node
sudo rm -rf /opt/local/lib/node_modules
```



## nvm

接下里就可以安装nvm啦～

参考nvm的官网安装教程，我的安装步骤是：

**1. 打开终端**

**2. 使用cURL：**

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.9/install.sh | bash
```

**3. **

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

**4. **

```
command -v nvm
```

这个时候terminal应该会输入nvm表示已经安装成功啦。

> 如果没有输入上面这行命令以后，没有反应，或者输出nvm: command not found。
>
> 官方给出的解决方法是：
>
> - your system may not have a [`.bash_profile file`] where the command is set up. Simply create one with `touch ~/.bash_profile` and run the install script again
> - you might need to restart your terminal instance. Try opening a new tab/window in your terminal and retry.

我就是出现了第一条的问题，输入touch touch ~/.bash_profile，再重新执行一遍安装流程就好啦～

**5. 安装node的淘宝镜像，加快下载速度**

```
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
```

**6. node的安装**

```
nvm install node
```

会直接安装最新版本的node，更多nvm的操作可以看官网文档～

**7. 安装npm的淘宝镜像**

```
npm config set registry https://registry.npm.taobao.org
```



## 最终目的

愉快的敲出react官网给出的安装命令！

```
npm install -g create-react-app
create-react-app my-app

cd my-app
npm start
```

成功get！



## 参考链接

+ 使用 nvm 管理不同版本的 node 与 npm：http://bubkoo.com/2017/01/08/quick-tip-multiple-versions-node-nvm/
+ nvm github：https://github.com/creationix/nvm/blob/master/README.md#installation
+ 镜像：https://segmentfault.com/a/1190000008410558