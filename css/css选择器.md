这两天看到一个下拉框展开的动画，用css3实现的，实现方式很简单，但是倒是让我复习了一下css选择器的用法～

## 原理分析

+ 原理就是父元素增加一条属性overflow：hidden，然后根据开关键，改变父元素的height值，看起来就有一种展开收缩的效果啦～

+ html代码如下：

```html
<h1><mark>PURE CSS</mark> Slide Up and Slide Down</h1>
<input id="toggle" type="checkbox"><label for="toggle">toggle slider</label>
<div id="wrap">
    <div id="slider">
        <p>Blah blah blah blah blah....</p>
    </div>
</div>
```

+ css代码如下：

```css
#wrap {
    height: 200px;
    width: 200px;
    border: 1px solid #ccc;
}
#slider {
    overflow-y: hidden;
    max-height: 200px;
    -webkit-transition: all .5s cubic-bezier(0, 1, 0.5, 1);
    transition: all .5s cubic-bezier(0, 1, 0.5, 1);
    background: pink;
    height: inherit; 
    width: inherit;
}
#toggle {
    display: none;
}
#toggle + label {
    padding: 1px 6px;
    text-align: center;
    border-radius: 2px;
    background: rgb(221, 221, 221);
    border: 1px outset buttonface;
    margin-bottom: 2px;
    display: inline-block;
    cursor: pointer;
}
/* most important part is here: */
#toggle:checked ~ #wrap > #slider {
    max-height: 0;
}
```

+ 效果为：

  ![](https://raw.githubusercontent.com/dy21335/Practice/master/src/toggleSlider.gif)

## css选择器

>  上面主要用到的选择器是，以下是对大部分选择器的总结～

1. 通用选择器：`*`设置所有元素；

2. 标签选择器：当为多个标签设置同一个样式时，各个标签用`,`隔开；

3. 类选择器是: `.class`；

   + 类选择器与元素标签相结合还可以制定不一样的属性：

   ```css
   p.warning {font-weight: bold;}
   span.warning {font-style: italic;}
   ```

   + 虽然类名一样，但是字体样式只适用于各自，不会泄露给对方。
   + 类选择器可以结合多个，像这种：

   ```
   p.warning.help{background:red;}
   ```

   只会匹配到同时包含（可以再有别的类）.warining和.help类p，而不会把样式用于只有一个样式的p；

4. `id`选择器以`#`开头，id只能被一个标签使用；

   + 注意类选择器和id选择器都是大小写敏感的；

5. 属性选择器：

   + 属性选择器是用`[]`；
   + 属性选择器可以多个结合，样式匹配的时候也只会匹配到包含这些所有属性的标签，比如：

   ```css
   a[href][title] {font-weight: bold;}
   //结果是只会选中下面的第一行：
   <a href="http://www.w3.org/" title="W3C Home">W3C</a><br />
   <a href="http://www.webstandards.org">Standards Info</a><br />
   <a title="Not a link">dead.letter</a>
   ```

7.属性+值选择器，这个是对确切的属性和值进行选择，也是用[]比如：

```css
a[href="http://www.css-discuss.org/about.html"] {font-weight: bold;}
```

这个也可以多个一起结合；

8. 属性+部分值选择器：

   + 属性+值选择器还可以跟`id`或者`class`属性选择哦；属性+值选择器一般都要把属性和值完整的写出来，但是，一个标签如果拥有多个类嘛，可以这么写：

     ```css
     p[class~="warning"] {font-weight: bold;}
     //加一个~，表示选择器基于的是一个属性值中有空格符号隔开的单词,会选中如下元素：
     <p class="urgent warning">When </p>
     ```

   + 还有一些其他的用法：还有一些其他的用法：

     ![img](data:image/png;base64,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) 

   + 还有一种属性选择器：

     ```css
     *[lang|="en"] {color: white;}
     //筛选出属性值等于en或从en-开始的值；
     ```

9. 后代选择器:

   + 后代选择器中，后代与子代之间用空格隔开，如果写的是：`ul em`，则会找到`ul`后代中的`em`标签，不论`em`被嵌套的有多深；

10. 孩子选择器：

    用连接符` >` 表示只选择孩子；

11. 相邻兄弟选择器：

    相邻兄弟选择器用连接符`>`，可选择紧接在另一元素后的元素，且二者有相同父元素。

    ```css
    div+p {background-color:yellow; }
    //以上实例选取了所有位于 <div> 元素后的第一个 <p> 元素:
    ```

12. 后续兄弟选择器：

    后续兄弟选择器用连接`~`，符选取所有指定元素之后的相邻兄弟元素。

    ```css
    div~p {   background-color:yellow; }
    //以上实例选取了所有 <div> 元素之后的所有相邻兄弟元素 <p> : 
    ```

13. 伪类选择器

    CSS规定 `:` 用来分割标签和伪类：

    ```css
    a:visited {color: red;}
    //上面这个例子表示a标签被点击后，变成红色；
    ```

14. `:first-child `被用于选择第一个孩子的伪类；这个其实表达的意思是冒号:前面的元素是它的父类的第一个子孩子。比如：

    `p:first-child`，其实选择的是`p`这个元素。它表示`p`是它父类的第一个子孩子。

15. 特殊值相加可以决定样式的优先级

    + ID选择器的特殊值被表示成 0,1,0,0；
    + class选择器、属性选择器、伪类选择器的特殊值被表示成 0,0,1,0；
    + 元素、伪元素选择器被表示成 0,0,0,1；
    + 连接符和通用选择器木有；



## Last

完结撒花～～