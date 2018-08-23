### 关于JSONP的使用

本次文章主要讲解运用jsonp实现跨域的原理和一个小例子的实现～

#### 什么是跨域？

要说什么是跨域，首先要说说什么是同源策略？

> 同源策略/SOP（Same origin policy）是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到XSS、CSFR等攻击。所谓同源是指**"协议+域名+端口"**三者相同，即便两个不同的域名指向同一个ip地址，也非同源。

所以要实现不用源的数据请求就要实现跨域啦～

#### 怎么实现跨域？

关于跨域的实现方式，网上的讲解有很多，大概就是：

+ jsonp
+ document.domain+iframe
+ location.hash+iframe
+ Window.name+iframe
+ HTML5的postMessage方法
+ CROS

附上一个我觉得跨域讲的还不错的网站：

https://www.cnblogs.com/roam/p/7520433.html

#### 怎么实现jsonp的跨域？

虽然网上讲解的很清楚，但是都没有实际代码来验证一下，所以以下讲解了一个可以实现**看到效果**的实际操作～

##### 原理

> jsonp主要是通过script标签的url属性（因此也只能实现get请求）；
>
> 因为通过script标签引入的js是不受同源策略的限制的；
>
> 所以我们可以通过script标签引入一个js或者是一个其他后缀形式（如php，jsp等）的文件，此文件返回一个js函数的调用（跟服务器约好的回调函数）。

##### 实例操作：

+ 跟服务器约定好一个回调函数；

我们先上网去找一下可以实现jsonp的网站接口：

https://www.bejson.com/knownjson/webInterface/

![a](https://raw.githubusercontent.com/dy21335/algorithm/master/pics/jsonp1.jpg)



这个网站有很多接口哦，我们选用第一个电商接口。

点击用例，把url参数修改为卫衣，网页显示了他返回的结果；

![b](https://raw.githubusercontent.com/dy21335/algorithm/master/pics/jsonp2.jpg)



![c](https://raw.githubusercontent.com/dy21335/algorithm/master/pics/jsonp4.jpg)

可见他的回调函数是`cb`，返回一个对象，里面又个`result`数组，接下来我们要实习的就是自己获取这个result数组，并在控制台打印出数组的第一个结果；

+ 把这个回调函数`cb`作为参数传递给服务器，服务器会返回一段js，这段js会调用写在客户端的回调函数，并往里面传参。

以下是js代码：

```js
function jsonp(url) {
    //动态生成一个script标签
    let script = document.createElement('script');
    script.src = url;
    script.setAttribute("type","text/javascript");
    document.body.appendChild(script);
}

function cb(data) {
    //由于该网址返回的就是一个对象，所有不用在解析啦
    //var obj = JSON.parse(data);

    console.log(data.result[0]);
}


window.onload = function (ev) {
    //给script标签传入url
    var url = 'http://suggest.taobao.com/sug?code=utf-8&q=' + '卫衣' +'&callback=cb';
    jsonp(url);//服务器会返回一段js，这段js是cb({result:[[...]...]});
}
```

+ 将上面的js文件插入到html中，在chrome浏览器的开发者工具的console里就可看到结果啦～

![d](https://raw.githubusercontent.com/dy21335/algorithm/master/pics/jp5.jpg)

#### 结语

具体代码都放在github的jsonp文件里了～：https://github.com/dy21335/Practice



