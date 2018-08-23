# css三角形

了解之前以为要用什么高超的绘图技巧，没想到这么简单。

### idea

+ 想法就是用`div`即可实现；
+ 将`div`的长和宽设置为0，而箭头的大小其实就是由`border`的`width`决定的；



### 实现

+ 将div的长宽设置为0，给border设置个200px的宽度，得到效果如下：

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1frrzzvx9eyj20uo0q474s.jpg)

**html**

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1frs0ekjd16j20h400ydfs.jpg)

**css**

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1frs0f9hvupj20jm07m0tt.jpg)



+ 由上图可以看到，其实`border`就是三角形，那么如果我们想要得到一个上三角，就可以把上左右的三个边框变成透明

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1frs0hkgkv6j20r00dg74d.jpg)



**css**

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1frs0ih3qrej20jm082wfq.jpg)





### 参考链接

https://css-tricks.com/snippets/css/css-triangle/