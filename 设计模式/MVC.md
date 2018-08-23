# MVC爬坑之旅

这是一篇长文。。

本来是想看MVVM模式，看到了MVC模式，就想着自己实现一下，真是看着简单，实现难，道理都懂，怎么还是走不好这条路？



写了一天才把逻辑理清楚，看来以前是似懂非懂，半懂不懂。。



私以为实现一个小栗子是理解的最好方式，接下来就跟着我看看我的栗子是怎么运用MVC模式的吧～



以下讲解分为：

+ 订阅者-发布者模式；

+ MVC模式；
+ 栗子君报道～



#### 要实现的效果为：

在下面的input写入一个城市，点击add按钮，将之添加到上面select元素中。

![MVC](https://raw.githubusercontent.com/dy21335/Practice/master/MVC/MVC.gif)





## 订阅者发布者模式

在实现MVC模式之前，我们先来看一会儿会用到的订阅者发布者模式；

+ 定义了一种一对多的关系；
+ 当一个对象的状态发生改变时，所有依赖它的对象都将得到通知；
+ 比如买房子，选购者都去售楼小姐那里登记一下，等有房源的时候，售楼小姐就通知所有登记的选购者过来看房；



## MVC模式



一图解所有：

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1frj887ty10j20cy0bojro.jpg)





#### mvc三要素

由上面的图也可以看出来：

+ 视图（view）： 用户界面
+ 控制器（controller）：业务逻辑
+ 模型（model）：数据保存

#### 他们的沟通

1. 用户输入给view；
2. view传送指令给controller，告诉controller，界面改变啦；
3. controller完成业务逻辑后，要求model改变状态；
4. model改变后，将新的数据发送给view，用户得到反馈。



## 栗子君报道～

前面都是初略一讲，主要还是要结合实际来理解啦～

+ view ：
  + 我们的应用程序在这部分可以访问DOM，并负责设置事件处理程序，例如：click事件；
  + view还负责呈现HTML，我们的栗子中将负责向用户显示城市列表；
  + 每当用户通过输入字段输入新任务时，view将会通知controller控制器更新模型。
+ model：
  + 这是为应用程序存储数据的位置；
  + 当model发生变化，它都会通知view更新视图；
  + model通知view的方式，我的实现是使用订阅者发布者模式；模型是发布者，视图是订阅者。

+ controller：

  + 控制器是模型和视图之间的粘合剂；
  + 当用户操作视图，它就会更新模型。在这个栗子里，我将在controller中实现model作为发布者，添加view作为订阅者，从而将model与view连接起来。

+ 事件分派器：

  + 事件分派器是一个对像；
  + 当我们调用该Event对象的notify方法时，将会运行我们附加到该Event的update方法。



### 首先是html界面～

样式在本栗中不是很重要，就不赘述啦，文末会有github链接，里面有完整代码

```javascript
<body>
    <select id="citySel" size="10"> </select>
        <input id="cityInput" type="text" value="">
        <button id="addBtn">  Add  </button>
</body>
```



###负责事件分发的Event类

```javascript
var Event = function (sender) {
    this._sender = sender;
    this._listeners = [];
}

Event.prototype = {

    attach: function (listener) {
        this._listeners.push(listener);
    },

    notify: function (args) {
        for (var i = 0; i < this._listeners.length; i += 1) {
            this._listeners[i].update(args);
        }
    }

};
```

+ 构造函数需要传入一个发布者，一会儿可以看到我在model中，将model作为了发布者；
+ attach方法用于添加监听者；
+ notify就是提供给发布者的方法；



### model

```javascript
/*model用于存储数据*/

var Model = {

    CityList : ['广州','深圳','珠海'],
    
	//实现发布者
    addCityEvent : new Event(this),

    addCityM : function (city) {
        this.CityList.push(city);
        this.addCityEvent.notify(city);
    },

    getCity: function () {
        return this.CityList;
    }

}
```



### view

```javascript

/*视图的展示*/

var CityView = {


    init: function (controller) {
        this.select = document.getElementById('citySel');
        this.addBtn = document.getElementById('addBtn');
        this.cityInput = document.getElementById('cityInput');
        this.controller = controller;
        this.addBtnHandler = this.addBtnHandler.bind(this);

	this.addBtn.addEventListener('click',this.addBtnHandler);

        this.render();
    },

    addBtnHandler : function () {
        //当要添加数据时，调用controller与model进行通信；
this.controller.incrementCity(this.cityInput.value.trim());
        this.cityInput.value ="";
    },

    createOption:function (data){
        var parent = this.select;
        for(var i=0; i<data.length; i++){
            var opt = document.createElement('option');
            //设置option的值
            opt.innerHTML = data[i];
            //定义option的自定义值
            parent.appendChild(opt);
        }
    },

    update: function(data){
        if(data == '' || data == undefined || data == null){
            console.log("kong");
        }
        else{
            var parent = this.select;
            var opt = document.createElement('option');
            //设置option的值
            opt.value = data;
            opt.appendChild(document.createTextNode(data));
            console.log(opt.innerHTML);
            //定义option的自定义值
            parent.appendChild(opt);
        }


    },

    render : function () {
        this.createOption(this.controller.getCurrentCity());
    }

}

```

+ view主要提供渲染视图的render()方法和更新视图的update()方法；
+ 主要在于addBtnHandler方法，当要向视图添加数据时，通知controller（调用controller的方法），让controller通知model更新数据；



### controller

```javascript
var Controller = {

    init : function (view, model){
        this.view = view;
        this.model = model;

        this.view.init(this);
        //将view作为订阅者
        this.model.addCityEvent.attach(this.view);
    },

    incrementCity: function(city) {
        this.model.addCityM(city);
    },

    getCurrentCity: function () {
        return this.model.getCity();
    }
};

```

+ 对view和model的初始化（本栗中model不需要init（）方法）；
+ `this.model.addCityEvent.attach(this.view);`这一步将view和model联系起来，使得model可以通知view改变；



### 最后一步

```javascript
Controller.init(CityView, Model);
```



### 体验

+ emmmmmmmm，体验不太好，可以看得出来view的部分繁重；
+ 感觉各部分耦合比较重；
+ 所以看了MVVM模式，明天打算实现一下～



## github实现链接

[mvc TEST](https://github.com/dy21335/Practice/tree/master/MVC)