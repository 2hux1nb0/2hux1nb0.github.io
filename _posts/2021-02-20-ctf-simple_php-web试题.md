---

layout: post
title:  "ctf-simple_php"
categories: [ctf]
tags: [php弱类型]

---

# simple_php

## **[原理]**

php中有两种比较符号

=== 会同时比较字符串的值和类型

== 会先将字符串换成相同类型，再作比较，属于弱类型比较

**需要注意的是：第六行if条件语句，如果改成if($a and $a==0)更好理解，我昨天卡在这懵了一个晚上，绕不过来弯，该语句内容可替换为$a==='0',或者更好地写法是上面我所说的改成单a在前，大佬当然别喷我，我只是碰到了这个小白可能也会碰到这个问题，绕不过来弯，我在这想多写一些解析的话给同样在这懵逼的人，这句话的语义是，如果 赋值给$a的那个字符串被转为数字后等于0 且 带上a的本身，满足这些条件的都能执行下面的内容，这么解释不知道你能不能明白，就是它后面加个and $a 这样写代码逻辑更完美，当然我仍然认为如果它换个顺序表达更好理解，是更好地写法**

## **[目地]**

掌握php的弱类型比较

## **[环境]**

windows

## **[工具]**

firefox

## **[步骤]**

1.打开页面，进行代码审计，发现同时满足 $a==0 和 $a 时，显示flag1。

2.php中的弱类型比较会使'abc' == 0为真，所以输入a=abc时，可得到flag1，如图所示。（abc可换成任意字符）。

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/simple_php/1.png)

3.is_numeric() 函数会判断如果是数字和数字字符串则返回 TRUE，否则返回 FALSE,且php中弱类型比较时，会使('1234a' == 1234)为真，所以当输入a=abc&b=1235a，可得到flag2，如图所示。

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/simple_php/2.png)