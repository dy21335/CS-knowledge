## dotLoading组件的实现

最近写了个用vue实现的Loading组件。使用组件的方式实现，易于拓展和维护，可以根据不同的需要来改变其大小和颜色。

实现的方式并不复杂，动画部分的实现使用的CSS3 animation，以后再用canvas实现一遍~

最终效果如下：

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1fsk76pb0c1j204403ja9u.jpg)



<br>



## 实现原理

+ 假如要实现四个圆的效果。其实就是要找个原点，以该点为圆心，均匀的画四个圆；

+ 如何确定这四个圆的位置呢？

  一个圆的弧度是2π，则第i个圆的对应的弧度是：

  ```
  rad = Math.PI *2/4*i
  ```

  那么他们对应的直角坐标系中的坐标为：

  ```javascript
   //radius为外圆，即下图黑色虚线圆的半径；
  dotX = Math.cos(rad) * radius
  dotY = Math.sin(rad) * radius
  ```

  

  如下图：

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1fsnp5xjhktj20ca0gz0sw.jpg)



<br>



## 具体实现

+ 将圆心用`ul`元素实现，放在父级`div`的正中间，其他圆点以`ul`的子元素`li`实现;

+ 模板部分参考代码:

  ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1fsnpkd0girj20np04gmxb.jpg)

  <br>

+ dotLoading组件，可以接受外部传递的参数：圆的直径，颜色，个数，整个外围圆的半径，通过他们可以确定该组件的大小和样式；

+ 需要传递给vue组件的参数，通过props的方式传递；

  ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1fsnpuvyio9j209t0dn0t0.jpg)

  <br>

+ 在获得props传递的size后，计算每个`li`(li的布局方式为绝对布局)`top`和`left`值，将其记录进`dotsStyle`数组，并在模板中，以`<li v-for="(dotStyle , index) in dotsStyle" :key="index" :style = "dotStyle" ></li>`的方式把样式赋给`li`：

  ```javascript
      dotStyle (size, color) {
        var fixStr
        var rad
        var dotX
        var dotY
        for (let i = 0; i < this.numbers; ++i) {
          //  计算弧度
          rad = Math.PI * 360 / this.numbers * i / 180
          //  计算相对于圆心的x和y坐标
          dotX = Math.cos(rad) * this.radius
          dotY = Math.sin(rad) * this.radius
          fixStr = {width: size, height: size, borderRadius: size, top: -dotY + 'px', left: dotX + 'px', backgroundColor: this.color, animationDelay: 1 / this.numbers * i + 's'}
          this.dotsStyle.push(Object.assign({}, fixStr))
          // console.log(`第${i}个圆点对应的坐标为：${dotX},${dotY}`)
        }
      }
  ```

<br>

+ 最后css动画部分：

  ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1fsnt1mvwgwj20jm0ka0ut.jpg)



  <br>


## 结语

以上就是我的想法啦～完整代码请参阅：

https://github.com/dy21335/my-component/blob/master/src/components/dotLoading.vue