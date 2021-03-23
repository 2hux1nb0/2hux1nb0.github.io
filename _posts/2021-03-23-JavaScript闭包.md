---

layout: post
title:  "JavaScript闭包"
categories: [原理定义]
tags: [JavaScript]

---

今天系统的学习了一遍JavaScript教程，发现一个很有意思的js用法------闭包。  

对于闭包的理解 我刚开始也很懵，其实就是一个大函数里面包了一个函数，外部的大函数的参数是里面的函数，返回值也是里面的函数值，那么通过这种方式，就可以将里面那个函数的变量变为外部大函数的私有变量，可谓是一个很骚的操作哈哈哈，也可以说是个bug。  

评论说Js之父发明的时候可能都想不到可以这么用，那肯定的哈哈



```
var add = (function () {
    var counter = 0;
    return function () {return counter += 1;}
})();
```

