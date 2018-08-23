## js的二维数组

之前轻视了js二维数组的问题，直到写笔试题的时候，才发现踩了不该踩的坑，所以今天做一下总结，以防再犯；

### 起因

起因是这样的，我想创建一个初值都为1的*4\*4*二维数组，于是乎很直接进行了如下操作：

```javascript
var d_arr = [];
for(let i = 0;i < 4;i++){
  for(let j = 0;j < 4;j++){
    d_arr[i][j] = 1;
  }
}
console.log(d_arr);
```

结果却一直报错

> TypeError: Cannot set property '0' of undefined



### 经过

思索了一会儿，我又修改为：

```javascript
var d_arr = [];
var temp_arr = [];
    for(let j = 0;j<4;j++){
        temp_arr[j] = 0;
    }
    for(let i = 0;i<4;i++){
        d_arr[i] = temp_arr;
    }
    console.log(d_arr);
```

不知道大家有没有看出来，我犯了一个很愚蠢的错误。。虽然console.log可以正常打印二维数组了，但是，每当我修改这个二维数组的值，比如我想修改\[0]\[1]值为0，当我进行`d_arr[0][1]=0；`后，结果是：`[ [ 0, 1, 0, 0 ], [ 0, 1, 0, 0 ], [ 0, 1, 0, 0 ], [ 0, 1, 0, 0 ] ]`    可见，如果看成这是一个4*4的矩阵，第二列全被改成了1，其实这是因为，数组是引用类型的，我赋值的时候，是把**同一个数组**赋给了d_arr的每一个元素。。d_arr的每一维都指向的是同一个对象呢。。



### 结果

虽然上面的修改也可以得到正确结果，但总是看着不正规，上网搜索，发现真正的操作方法是我一直忽略的new Array()!!

```javascript
var d_arr = new Array();
for(let i = 0;i<4;i++){
  d_arr[i] = new Array();
  for(let j = 0;j<4;j++){
    d_arr[i][j] = 0;
  }
}
console.log(d_arr);
```

这样就是一个想要得到的d_arr啦！

ps：new Array还有一个用处，就是指定数组的长度：`var arr = new Array(8);`