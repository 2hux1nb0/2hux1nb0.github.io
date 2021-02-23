---

layout: post
title:  "ctf-command_execution命令拼接"
categories: [ctf]
tags: [命令拼接]

---



# command_execution

## **[原理]**

> | 的作用为将前一个命令的结果传递给后一个命令作为输入

> &&的作用是前一条命令执行成功时，才执行后一条命令
>

## **[目地]**

掌握命令拼接的方法

## **[环境]**

windows

## **[工具]**

firefox

## **[步骤]**

> 1.打开浏览器，在文本框内输入127.0.0.1 | find / -name "flag.txt" （将 | 替换成 & 或 && 都可以）,查找flag所在位置，如图所示。

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/command_execution/1.png)

> 2.在文本框内输入 127.0.0.1 | cat /home/flag.txt 可得到flag，如图所示。

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/command_execution/2.png)