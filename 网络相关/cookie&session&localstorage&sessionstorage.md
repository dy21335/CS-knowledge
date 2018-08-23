http://harttle.land/2015/08/16/localstorage-sessionstorage-cookie.html

https://www.jianshu.com/p/78f65023cf9d

cookie，session，localstorage，SessionStorage这几个词经常出现，本文是融合了网上几篇文章后得出的总结，参考链接都在上面给出了～



## cookie

### 引言

+ HTTP是一个无状态的协议，也就是每次发送一个请求，建立一次链接，下次再发送请求时，重新建立连接。
+ 比如说你先登录到淘宝页面，然后关掉了。不多久你又开到了淘宝，你还得输入用户名密码，因为http不会记录你的状态。
+ 浏览器和服务器之间有约定：通过使用cookie技术来维护应用的状态。 Cookie 能够帮助 Web 站点**保存有关访问者的信息**。比如上面的栗子，有了cookie后，你可以在再次打开淘宝时自动登录，省去了你输入用户名密码的步骤。



### 进入正题

#### 1. 细说cookie是怎么起作用的？

+  cookie 是存在**客户端**的（会话cookie一般不存储在硬盘上而是保存在内存里，当然这种行为并不是规范规定的。若设置了过期时间，浏览器就会把cookie保存到硬盘上，关闭后再次打开浏览器，这些cookie仍然有效直到超过设定的过期时间。）**每次**HTTP请求的时候，客户端都会发送相应的Cookie信息到服务端。
+ 当用户访问一个站点时，浏览器就会在本地硬盘上查找与该 URL 相**关联**的 `Cookie`。如果该 Cookie 存在，浏览器就将它添加到`request header`的`Cookie`字段中，与`http请求`一起发送到该站点。
+ 由于存储在`cookie`的数据「每次」都会被浏览器「自动」放在`http`请求中。因此，如果这些数据不是每次都要发给服务器的话，这样做无疑会增加网络流量，这也是cookie的缺点之一。为了避免这点，我们必须考虑什么样的数据才应该放在`cookie`中，而不是滥用cookie。每次请求都需要携带的信息，最典型的就是身份验证了，其他的大多信息都不适合放在cookie中。



#### 2. cookie的参数

+ **expires**  解释cookie在什么时间段内有效；
+ **domain**和**path**  限制cookie能被那些网站访问；
+ **secure**  设置`cookie`只在确保安全的请求中才会发送(https等安全协议)；
+ **httpOnly**  设置cookie能否通过js来访问，拥有httpOnly的cookie只能被服务器访问，客户端不能访问，可以用来防止**XSS脚本攻击**。



#### 3. cookie的父子作用域

news.164.com和sports.163.com是163.com域名下的子域。

+ 当 Cookie 的 domain 为 `news.163.com`，那么访问 `news.163.com` 的时候就会带上 Cookie；
+ 当 Cookie 的 domain 为 `163.com`，那么访问 `news.163.com` 和 `sports.163.com`就会带上 Cookie。



#### 5 . cookie的大小

+ cookie的存放有大小限制，不可以超过4KB, 每一个域名下的cookie数量最多是20个。





## session

### 引言

- session存放在**服务端**，类似散列表的形势，每一个session有一个sessionId。比如淘宝的购物车的内容，服务器根据session ID来返回不同用户的不同购物车内容。
- 如果没有sessionID，在服务端会新建一个sessionId，然后返回给客户端。
- 客户端发起请求的时候会带上sessionID，实际上大多数的应用都是用 Cookie 来实现Session跟踪的，第一次创建Session的时候，服务端会在HTTP协议中告诉客户端，需要在 Cookie 里面记录一个Session ID，以后每次请求把这个会话ID发送到服务器，我就知道你是谁了。



### 举个栗子：登录过程

- 首次提交用户密码**以及记住密码选项**到服务器,从数据库验证,成功返回两个cookie,一个是session_id的cookie,一个是用于下次登录免输入密码的cookie;
- 登录之后浏览网页,每次都会携带session_id到服务器,验证身份.如果服务器设置了session过期时间为2个小时,那么两个小时之后,你的账号就会强制被退出了,因为你的session过期了.
- 下一次登录的时候,浏览器会携带上次账户密码登录获取到的cookie,服务器对这个cookie进行验证,成功的话就直接登录,失败就进行账户密码的登录.一般说的cookie泄露是指这个cookie泄露.



## cookie&session的异同

他俩的异同，我觉得https://juejin.im/entry/5766c29d6be3ff006a31b84e这个网站写的很清楚，由于篇幅比较长，就不再这里展开啦～

我觉得值得注意的是，session不支持父子跨域，而cookie是子域可以携带父域的cookie。

还有服务器的压力：

> Session是保管在服务器端的，每个用户都会产生一个Session。假如并发访问的用户十分多，会产生十分多的Session，耗费大量的内存。因而像Google、Baidu、Sina这样并发访问量极高的网站，是不太可能运用Session来追踪客户会话的。

而Cookie保管在客户端，不占用服务器资源。假如并发阅读的用户十分多，Cookie是很好的选择。关于Google、Baidu、Sina来说，Cookie或许是唯一的选择。



## localstorge&sessionStorage

+ **LocalStorage**和**SessionStorage**都是本地存储，不会被发送到服务器上。同时空间比Cookie大很多，一般支持**5-10M**。 与Cookie类似，每个域名下会有不同的localStorage和SessionStorage实例，而且localStorage可以在多个标签页中互相访问。 其中LocalStorage没有过期时间，除非手动删除它会一直存在。而SessionStorage在浏览器会话结束时（关闭标签页，不包括刷新和跳转）清空。
+ localStorage是html5提出的。



## sessionStorage 、localStorage 和 cookie 之间区别

一下总结来自：https://www.geekjc.com/post/58e10a3867d6027b0d916752

+ 共同点：都是保存在浏览器端，且同源的。
+ 区别：
  + cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。
  + 存储大小限制也不同，cookie数据不能超过**4k**，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。
  + 数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。
  + 作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的。
  + Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。
  + Web Storage 的 api 接口使用更方便。著作权归作者所有。





## 总结

- 服务端存储：session
- 浏览器端存储：cookie，localStorage，sessionStorage