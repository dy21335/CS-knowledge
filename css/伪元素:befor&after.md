##奇妙的CSS之旅：有用的伪元素

最近在用css写动画，发现了一些好玩的效果～决定以后把遇到的有趣特效记录下来，供自己回顾与大家参考～

**今天主要讲伪元素:before 和 :after的使用。**

### 1. 基本概念

> “伪元素”，顾名思义。它创建了一个虚假的元素，并插入到目标元素内容之前或之后。

伪元素添加不会出现在 DOM 中，不可复制，仅仅是在 CSS 渲染层加入。

                           

### 2. 基本语法

```css
p:before {}

p:after {}
```

在 CSS3 中为了区别伪元素和伪类为伪元素使用了双冒号，因此如果使用了 display 或者 width 等属性时使得显示脱离了原本元素后，建议按照标准双写。

                      

**属性content**

这两个伪类下特有的属性` content` ，用于在 CSS 渲染中向元素逻辑上的头部或尾部添加内容。

+ 伪元素如果没有设置“content”属性，伪元素是无用的。可以设置content属性值为空，仅仅把他当做一个盒子。
+ content属性可以有以下几类值：

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftd8fcvlf7j215u0q0q8e.jpg)

                      

**插入样式**

+ 伪元素相对于主元素进行定位；
+ 插入的元素在默认情况下是[内联元素](http://www.w3.org/TR/html401/struct/global.html#h-7.5.3)。因此，为了给插入的元素赋予高度，填充，边距等等，你通常必须显式地定义它是一个块级元素。

```css
#element:before{
    content: "";
    display:block;
    width:200px;
    height:200px;
}
```

                      

###  3.奇妙效果

知道了伪元素的概念和用法当然是为了使用它啦～

**展开边框**

![](https://raw.githubusercontent.com/dy21335/Practice/master/src/pseudo1.gif)



**飞入边框**

![](https://raw.githubusercontent.com/dy21335/Practice/master/src/pseudo2.gif)



**悬浮方括号**

![](https://raw.githubusercontent.com/dy21335/Practice/master/src/pseudo3.gif)



                 

### 4.代码解析

这里接给出一个边框展开的代码，其他道理都是相通的～想看其他的可以查看后面给的GitHub链接～

```css
<style>
    .strech{
        position: relative;
        background-color: #FFF3E0;
        width: 100px;
        height: 100px;
        margin: 30px auto;
    }
    .strech:before{
        transform: scaleX(0);
    }
    .strech:after{
        transform: scaleY(0);
    }

    .strech:before,.strech:after{
        content: "";
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        display: block;
        pointer-events: none;
    }
    .strech:hover::before{
        transform: scaleX(1);
        border-top: 1px solid #F0B27A;
        border-bottom: 1px solid #F0B27A;
    }
    .strech:hover::after{
        transform: scaleY(1);
        border-left: 1px solid #F0B27A;
        border-right: 1px solid #F0B27A;
    }
    .strech:hover::before,.strech:hover::after{
        transition: transform .2s linear;
    }
    </style>
```

```html
<body>
<div class="strech"></div>
</body>
```

+ 主要是伪元素结合transform实现效果；
+ 因为伪元素默认是内联元素，所以要设置`display:block`，才可设置宽高；
+ point-events属性，这是针对鼠标事件的属性，预设值为 auto，若值为 none，则可以穿越该元素，点击或hover到下方的元素。（设置这个属性以防伪元素遮盖主元素的鼠标事件）。



### GitHub链接：

https://github.com/dy21335/Practice/blob/master/html/pseudo.html