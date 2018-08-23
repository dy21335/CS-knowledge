bind()是es6才有的特性，那么如果浏览器不支持es6要怎么去实现呢？

以下是对下面给出的这个链接，仿造里面的内容，自己又实现了一遍。

https://github.com/mqyqingfeng/Blog/issues/12



# bind使用

>  bind() 方法会创建一个新函数。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，之后的一序列参数将会在传递的实参前传入作为它的参数。(来自于 MDN )

> 使用方法：fun.bind(thisArg[, arg1[, arg2[, ...]]])
>
> 返回值：fun函数的拷贝，和指定的this值，以及初始化的参数。

栗子（来自MDN）：

```javascript
var moudle = {
    x: 42,
    getX: function () {
        console.log(this.x);
    }
}

var retrieveX = moudle.getX;
retrieveX();//undefined,this指向是global

var boundGetX = retrieveX.bind(moudle);
boundGetX();//42，this指向是moudle
```





# bind的模拟实现



### 1.返回函数的模拟实现

bind函数第一个特点就是能返回一个新的函数，并且改变了这个新函数的this指向。

```javascript
var moudle = {
    x: 42,
    getX: function () {
        console.log(this.x);
    }
}

var retrieveX = moudle.getX;
retrieveX();//undefined,this指向是global



Function.prototype.bind2 = function (context) {
    var self = this;
    return function () {
        return self.apply(context);
    }
}

var boundGetX = retrieveX.bind2(moudle);
boundGetX();//42，this指向是moudle
```



这里之所以是`return self.apply(context)`，是因为有些绑定函数的是有返回值的。



### 2.传参的模拟实现

+ bind函数的第二个特点就是能够它创建的新函数传入参数。
+ 对于这一点，我们可以用arguments来处理。
+ bind不仅在bind时可以传参，在调用bind返回的函数时，还可以给函数传参哦～
+ **原bind效果**：

```javascript
var moudle = {
    x: 42,
    getX: function (name, age) {
        this.name = name;
        this.age = age;
        console.log(this.x);
        console.log(this.name);
        console.log(this.age);
    }
}


var retrieveX = moudle.getX;
retrieveX();//undefined,undefined,undefined
//第一次传参，传递name的值
var boundGetX = retrieveX.bind(moudle,"daisy");
//第二次传参，传递age的值
boundGetX(100); // 42，daisy, 100
```



+ **自己实现**：

```javascript
var moudle = {
    x: 42,
    getX: function (name, age) {
        this.name = name;
        this.age = age;
        console.log(this.x);
        console.log(this.name);
        console.log(this.age);
    }
}

var retrieveX = moudle.getX;
retrieveX();//undefined,undefined,undefined


Function.prototype.bind2 = function (context) {
    //只有一个参数context，context存放的是返回函数的this指向
    var self = this;
    //获取传入的参数里第二参数到最后一个参数的数组
    var args = Array.prototype.slice.call(arguments,1);

    return function () {
        // 这个时候的arguments是指bind返回的函数传入的参数
        var bindArgs = args.concat(Array.prototype.slice.call(arguments));

        return self.apply(context,bindArgs);
    }
}

var boundGetX = retrieveX.bind2(moudle,"sandra");
boundGetX(100);//42，sandra,100
```





> Ps:关于`Array.prototype.slice.call(arguments)`的使用:
>
> `Array.prototype.slice.call(arguments)`能将具有length属性的对象转成数组，除了IE下的节点集合。因为argumens是类数组对象，然后又没有给出截取的长度，则全部截取。



### 3.构造函数效果的模拟实现

+ bind的第三个特点就是：当bind返回的函数作为构造函数的时候，bind时指定的this值就会失效，但是参数依然有效。

> 一个绑定函数也能使用new操作符创建对象：这种行为就像把原函数当成构造器。提供的 this 值被忽略，同时调用时的参数被提供给模拟函数。

+ 原bind效果：

```javascript
var moudle = {
    x: 42,
    getX: function (name, age) {
        this.name = name;
        this.age = age;
        console.log(this.x);
        console.log(this.name);
        console.log(this.age);
    }
}


var retrieveX = moudle.getX;
var boundGetX = retrieveX.bind(moudle,"daisy",100);
//this的指向改变，改成obj，而obj对象的x值为undefied；
var obj = new boundGetX();//undefined,daisy,100
```



+ **自己实现**：

```javascript
var moudle = {
    x: 42,
    getX: function (name, age) {
        this.name = name;
        this.age = age;
        console.log(this.x);
        console.log(this.name);
        console.log(this.age);
    }
}


var retrieveX = moudle.getX;
retrieveX();//undefined,undefined,undefined
// var boundGetX = retrieveX.bind(moudle,"daisy",100);
// var obj = new boundGetX();//undefined,daisy,100


Function.prototype.bind2 = function (context) {
    var self = this;
    //获取传入的参数里除去第一个参数之后的参数
    var args = Array.prototype.slice.call(arguments,1);

    var fBound = function () {
        // 这个时候的arguments是指bind返回的函数传入的参数
        var bindArgs = args.concat(Array.prototype.slice.call(arguments));
        //检查是否是把fBound当构造函数用？如果是构造函数，那么this值指向了当前这个new出来的obj；
        return self.apply(this instanceof fBound? this:context,bindArgs);
    }

    //// 修改返回函数的 prototype 为绑定函数的 prototype，实例就可以继承绑定函数的原型中的值;
    fBound.prototype = this.prototype;
    return fBound;
}

var boundGetX = retrieveX.bind2(moudle,"sandra",100);
var obj = new boundGetX();//undefined sandra 100
```



## 构造函数的优化实现

我们直接将 fBound.prototype = this.prototype，我们直接修改 fBound.prototype 的时候，也会直接修改绑定函数的 prototype。这个时候，我们可以通过一个空函数来进行中转：

```javascript
Function.prototype.bind2 = function (context) {
    var self = this;
    //获取传入的参数里除去第一个参数之后的参数
    var args = Array.prototype.slice.call(arguments,1);

    var fNop = function () {};

    var fBound = function () {
        // 这个时候的arguments是指bind返回的函数传入的参数
        var bindArgs = args.concat(Array.prototype.slice.call(arguments));
        //检查是否是把fBound当构造函数用？如果是构造函数，那么this值指向了当前这个new出来的obj；
        return self.apply(this instanceof fBound? this:context,bindArgs);
    }

    //// 修改返回函数的 prototype 为绑定函数的 prototype，实例就可以继承绑定函数的原型中的值
    fNop.prototype = this.prototype;
    fBound.prototype = new fNop();
    return fBound;
}
```



## 最后

MDN的polyfill为：

```javascript
if (!Function.prototype.bind) {
  Function.prototype.bind = function(oThis) {
    if (typeof this !== 'function') {
      // closest thing possible to the ECMAScript 5
      // internal IsCallable function
      throw new TypeError('Function.prototype.bind - what is trying to be bound is not callable');
    }

    var aArgs   = Array.prototype.slice.call(arguments, 1),
        fToBind = this,
        fNOP    = function() {},
        fBound  = function() {
          return fToBind.apply(this instanceof fNOP
                 ? this
                 : oThis,
                 aArgs.concat(Array.prototype.slice.call(arguments)));
        };

    if (this.prototype) {
      // Function.prototype doesn't have a prototype property
      fNOP.prototype = this.prototype; 
    }
    fBound.prototype = new fNOP();

    return fBound;
  };
}
```



