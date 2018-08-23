### webpack了解一下？

最近打算学习一下webpack，看到一篇博文跟着博文的内容走，却发现踩了好多坑。。查了半天资料，才发现是版本问题。。

原文教程可能有点过时了，不适合与webpack4，但是原文有些内容还是值得学习的，所以还是把链接放出来供大家参考～[参考链接](https://www.jianshu.com/p/42e11515c10f)

下面的讲解就是基于webpack4的一个新手入门操作啦～参考网址：https://appdividend.com/2018/03/10/webpack-4-tutorial-example-scratch/

本文算是对它的翻译和补充吧。

### 什么是webpack？

+ webpack分析你的项目结构，找到JavaScript模块以及其他一些浏览器不能直接运行的拓展语言，将其转换和打包为合适的格式供浏览器使用。

### webpack 初认识！

作为入门，我们只需要明白几个核心概念：

+ Entry
+ Output
+ Loaders
+ Plugins

#### Entry

entry表示了webpack应该从哪个模块作为切入来构建其内部依赖关系图，webpack会计算出哪些库和模块是入口点所依赖的；

```Js
//webpack.config.js
module.exports = {
    entry:'./path/to/my/entry.file.js'
}
```

#### Output

output属性告诉webpack向哪里发送你的*bundles*，并且他会在建立的时候命名这些文件；

#### Loaders

Loaders会让webpack可以处理js文件以外的类型，把他们转换成webpack可以处理的modules，比如vue-loader，babel-loader，css-loader等等；

#### Plugins

plugins可以执行更广泛的任务；



### 开始动手吧～

+ 首先创建一个**webpack-sample文件**，我们的项目就放在这个文件里；


+ 接下里进入webpack-sample文件：

```
npm init
npm install webpack webpack-cli -D
```

*关于第一行*

会创建一个package.json的文件，里面有一些你的项目依赖之类的，他会指定一个入口点，默认为**index.js**，这也是webpack4的新特性～；

*关于第二行*

安装webpack和webpack-cli包作为开发依赖；并且你会发现package.json里会增加：

```Js
  "devDependencies": {
    "webpack": "^4.4.1",
    "webpack-cli": "^2.0.13"
  }
```



插个题外话：

>`npm install -g webpack`表示是安装一个全局的webpack，而`npm install webpack`则是安装一个局部的webpack。上面的-D也是跟—save-dev是一个效果～

*npm局部和全局又啥区别呢?*

	全局环境下，包安装在**Node安装目录**下的node_modules文件夹中；

	局部环境下，安装在该**项目**的node_modules文件下；

	局部环境下，可以直接通过require（）的方式引入项目中的node_modules目录下的模块；

	默认情况下，node.js会在NODE_PATH和目前js所在项目下的node_modules文件夹下去寻找模块；如果是全局安装，不能直接require（）的方式去引用模块，需要手动解决包路径的配置问题；因此不推荐全局安装哦～

*再插个题外话*

有时候看一些博文会看到`—save-dev`和`--save`命令，下面写了解释～

> 1. `—save-dev`是你开发时候依赖的东西，`--save`是你发布之后还依赖的东西。比如ES6代码要编译成ES5，那么`babel`就是devDependencies。而jquery是发布以后还依赖的，就是dependencies；
> 2. npm install在安装npm包时，有两种参数可以把它们的信息写入`package.json`文件(一个标准的npm说明文件)，一个是npm install —save，另一个就是npm install —save-dev；
> 3. —save会把依赖包名称添加到package.json文件`dependencies`键下，—save-dev则添加到package.json的`devDependencies`键下；



+ 接下来创建一个**src文件**，在src文件中创建一个**index.js**，在里面随便写点东西：

  ```js
  console.log("hi~webpack");
  ```

+ 直接在命令行里运行`webpack`;会发现工程目录里多了一个dist文件夹，并且里面创建了一个main.js，默认的build是production，是意思？且看下面分解～

  + development和production模式有啥不一样？

    其实development模式就是更方便程序员阅读的模式，方便程序员在开发的时候调试代码。而 production就是最后项目被压缩过的代码。

    可以在package.json文件里的scripts属性里增加以下代码：

    ```Js
    "scripts": {
        "dev": "webpack --mode development",
        "prod": "webpack --mode production"
    },
    ```

    现在运行`npm run dev`就是启动development模式，运行`npm run prod`就是production模式啦～

    ​

### babel转换ES6语言

+ babel-lodaer就是webpack loader用来编译ES6语言的，我们需要安装它的环境依赖（可观察到package.json的devDependencies又多了些东西哦～）：

```javascript
npm install babel-core babel-loader -D
```

+ 在项目根目录创建一个**webpack.config.js**；
+ 定义**babel-loader**的参数：

```javascript
// webpack.config.js

module.exports = {
   module: {
      rules: [
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: {
            loader: "babel-loader"
          }
        }
      ]
   }
}
```



+ 再安装一个依赖,它提供给我们一个位于我们主机的开发服务器，也就是最后页面的地址local：8080：

  ```
  npm install webpack-dev-server -D
  ```

  修改package的参数：

  ```
  "scripts": {
      "dev": "webpack-dev-server --mode development",
      "prod": "webpack --mode production"
  },
  ```

+ 在跟目录创建一个**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Webpack 4 Tutorial</title>
</head>
<body>
   <div id="app"></div>
   <script src="main.js"></script>
</body>
</html>
```

+ 修改index.js里的内容：

```javascript
const app = (a, b) => {
   return a + b;
}

alert(app(4,5));
```

+ 现在执行npm run dev ～，打开http://localhost:8080/ 就可以看到效果啦～

### 结语

以上就是webpack的初入门体验啦～



