---

layout: post
title:  "ctf-simple_js & 十进制转字符串"
categories: [ctf]
tags: [JavaScript]

---

# simple_js

## **[原理]**

javascript的代码审计

## **[目地]**

掌握简单的javascript函数

## **[环境]**

windows

## **[工具]**

firefox

## **[步骤]**

1.打开页面，查看源代码，可以发现js代码，如图所示。

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/simple_js/1.png)

2.进行代码审计，发现不论输入什么都会跳到假密码，真密码位于 fromCharCode 。

3.先将字符串用python处理一下，得到数组[55,56,54,79,115,69,114,116,107,49,50]，exp如下。

```python
s="\x35\x35\x2c\x35\x36\x2c\x35\x34\x2c\x37\x39\x2c\x31\x31\x35\x2c\x36\x39\x2c\x31\x31\x34\x2c\x31\x31\x36\x2c\x31\x30\x37\x2c\x34\x39\x2c\x35\x30"
print (s)
```

4.将得到的数字分别进行ascii处理，可得到字符串786OsErtk12，exp如下。

```python
a = [55,56,54,79,115,69,114,116,107,49,50]
c = ""
for i in a:
    b = chr(i)
    c = c + b
print(c)
```

5.规范flag格式，可得到Cyberpeace{786OsErtk12}