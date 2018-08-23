以下来自对一篇外文的翻译。

http://www.stevesouders.com/blog/2009/04/09/dont-use-import/

# link vs @import

有两种方法可以把一个样式表包含进你的网页：

```html
<link rel='stylesheet' href='a.css'>
//或者
<style>
@import url('a.css');
</style>
```



####@import@import

栗子1:如果我们用@import来引入两个样式表：

```html
<style>
@import url('a.css');
@import url('b.css');
</style>
```

如果我们只用@import来引入文件，则不存在任何性能问题，两个样式表可以并行下载，但是当@import嵌入其他样式表或与link结合使用时则会出现问题。



#### link @import

栗子2:

```html
<link rel='stylesheet' type='text/css' href='a.css'>
<style>
@import url('b.css');
</style>
```

在IE中（在6,7和8上进行测试），这会导致样式表按顺序下载，也就是下载完a以后，再下载b。并行下载资源是更快页面的关键，在IE中的这种行为导致页面花费较长时间才能完成。

#### link with @import

栗子3:

```html
//in the HTML document
<link rel='stylesheet' type='text/css' href='a.css'>


//in a.css:
@import url('b.css');
```

这样在**所有浏览器中**都是顺序下载；



#### link blocks @import

栗子4:

in the HTML document:

```html
<link rel='stylesheet' type='text/css' href='a.css'>
<link rel='stylesheet' type='text/css' href='proxy.css'>
```

in proxy.css:

```html
@import url('b.css');
```

在IE中，proxy.css和a.css并行下载，但是b.css要等到a.css下载完才会开始下载；

在其他浏览器中，proxy.css和a.css并行下载，proxy.css解析到@import的文件，也马上开始并行下载；



#### many @imports

栗子5：

```html
<style>
@import url('a.css');
@import url('b.css');
@import url('c.css');
@import url('d.css');
@import url('e.css');
@import url('f.css');
</style>
<script src='one.js' type='text/javascript'></script>
```

即使script脚本是在最后列出的，它也会首先在IE中下载。 如果脚本包含的代码依赖于从样式表`a la getElementsByClassName`等应用的样式，则可能会出现意外的结果，因为脚本在样式表之前加载，尽管开发人员最后列出了它。



#### link link

栗子6:

```Html
<link rel='stylesheet' type='text/css' href='a.css'>
<link rel='stylesheet' type='text/css' href='b.css'>
```

这两个文件会并行下载



### 总结

+ 在样式表中使用@import让页面的整体下载时间增加，因为如果有link文件，它要等link文件下载完才开始下载。
+ 在IE中使用@import会导致下载顺序被更改（最后插入的脚本被最先执行）。这可能会导致样式表花费更长的时间下载，这会阻碍渲染使页面变得更慢的进度。
+ 加载顺序的差别。当一个页面被加载的时候（就是被浏览者浏览的时候），link引用的CSS会同时被加载，而@import引用的CSS 会等到页面全部被下载完再被加载。
+ link支持使用Javascript控制DOM去改变样式；而@import不支持。