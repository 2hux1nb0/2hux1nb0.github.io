---

layout: post
title:  "xff-referer伪造ip地址和域名"
categories: [ctf]
tags: [xff referer]

---

最新版的BurpSuite（ Pro v2021.2）与以前版本不同，将raw headers hex这些二级导航栏去掉，改在了右侧显示，需要Add伪造ip和域名的时候，在该部分右侧底部有add按钮等老版功能，进行添加操作和其他老版功能

![burpsuite](https://img-blog.csdnimg.cn/20210220111744850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzYzOTY4Mg==,size_16,color_FFFFFF,t_70#pic_center)

## X-Forwarded-For

- 是一个 HTTP 扩展头部，主要是为了让 Web 服务器获取访问用户的真实 IP 地址。在代理转发及反向代理中经常使用X-Forwarded-For 字段。
- 是用来识别通过HTTP代理或负载均衡方式连接到Web服务器的客户端最原始的IP地址的HTTP请求头字段。简单地说，xff是告诉服务器当前请求者的最终ip的http请求头字段，通常可以直接通过修改http头中的X-Forwarded-For字段来仿造请求的最终ip
- <https://www.cnblogs.com/huaxingtianxia/p/6369089.html>
- 产生背景

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316175855199.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316175908263.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hlX3N0YW5k,size_16,color_FFFFFF,t_70)

代理

- 正向代理

  正向代理是一个位于客户端【用户A】和原始服务器(origin server)【服务器B】之间的服务器【代理服务器Z】，为了从原始服务器取得内容，用户A向代理服务器Z发送一个请求并指定目标(服务器B)，然后代理服务器Z向服务器B转交请求并将获得的内容返回给客户端。

  作用：1. 访问本无法访问的服务器B

   2.加速访问服务器B

   3.Cache作用

   4.客户端访问授权

   5.隐藏访问者的行踪

- 反向代理

  反向代理正好与正向代理相反，对于客户端而言代理服务器就像是原始服务器，并且客户端不需要进行任何特别的设置。客户端向反向代理的命名空间(name-space)中的内容发送普通请求，接着反向代理将判断向何处(原始服务器)转交请求，并将获得的内容返回给客户端。

  作用：1、保护和隐藏原始资源服务器

   2、负载均衡

- 透明代理

  透明代理的意思是客户端根本不需要知道有代理服务器的存在，它改编你的request fields（报文），并会传送真实IP。注意，加密的透明代理则是属于匿名代理，意思是不用设置使用代理了。

<https://blog.csdn.net/championhengyi/article/details/80671517>



## Referer

[Referer ](https://www.sojson.com/tag_referer.html)是 [ HTTP ](https://www.sojson.com/tag_http.html)请求`header` 的一部分，当浏览器（或者模拟浏览器行为）向`web` 服务器发送请求的时候，头信息里有包含 [ Referer ](https://www.sojson.com/tag_referer.html)。比如我在`www.sojson.com` 里有一个`www.baidu.com` 链接，那么点击这个`www.baidu.com` ，它的`header` 信息里就有：

  Referer=https://www.sojson.com

由此可以看出来吧。它就是表示一个来源。看下图的一个请求的[ Referer ](https://www.sojson.com/tag_referer.html)信息。



![img](https://cdn.yinshua86.com/file/16-01-23-02-49-30/doc/6023914857.jpg)





### 这里有一个小问题要说明下。

[Referer ](https://www.sojson.com/tag_referer.html)的正确英语拼法是`referrer` 。由于早期HTTP规范的拼写错误，为了保持向后兼容就将错就错了。其它网络技术的规范企图修正此问题，使用正确拼法，所以目前拼法不统一。还有它第一个字母是大写。



### Referer的作用

1.防盗链。

刚刚前面有提到一个小[ Demo ](https://www.sojson.com/tag_demo.html)。

我在www.sojson.com里有一个`www.baidu.com`链接，那么点击这个`www.baidu.com`，它的header信息里就有：

> Referer=https://www.sojson.com

那么可以利用这个来防止盗链了，比如我只允许我自己的网站访问我自己的图片服务器，那我的域名是`www.sojson.com`，那么图片服务器每次取到Referer来判断一下是不是我自己的域名`www.sojson.com`，如果是就继续访问，不是就拦截。

这是不是就达到防盗链的效果了？

2.防止恶意请求。

比如我的SOJSON网站上，静态请求是`*.html`结尾的，动态请求是`*.shtml`，那么由此可以这么用，所有的`*.shtml`请求，必须[ Referer ](https://www.sojson.com/tag_referer.html)为我自己的网站。

> Referer=https://www.sojson.com

### 空Referer是怎么回事？什么情况下会出现Referer?

首先，我们对空[ Referer ](https://www.sojson.com/tag_referer.html)的定义为，[ Referer ](https://www.sojson.com/tag_referer.html)头部的内容为空，或者，一个[ HTTP ](https://www.sojson.com/tag_http.html)请求中根本不包含[ Referer ](https://www.sojson.com/tag_referer.html)头部。

那么什么时候[ HTTP ](https://www.sojson.com/tag_http.html)请求会不包含[ Referer ](https://www.sojson.com/tag_referer.html)字段呢？根据Referer的定义，它的作用是指示一个请求是从哪里链接过来，那么当一个请求并不是由链接触发产生的，那么自然也就不需要指定这个请求的链接来源。

比如，直接在浏览器的地址栏中输入一个资源的URL地址，那么这种请求是不会包含[ Referer ](https://www.sojson.com/tag_referer.html)字段的，因为这是一个“凭空产生”的[ HTTP ](https://www.sojson.com/tag_http.html)请求，并不是从一个地方链接过去的。

### 那么在防盗链设置中，允许空Referer和不允许空Referer有什么区别？

允许[ Referer ](https://www.sojson.com/tag_referer.html)为空，意味着你允许比如浏览器直接访问，就是空。

如下图：

![img](https://cdn.yinshua86.com/file/16-01-23-02-54-09/doc/6753290148.jpg)

这就是空的[ Referer ](https://www.sojson.com/tag_referer.html)。

拒绝空的[ Referer ](https://www.sojson.com/tag_referer.html)。比如我的`www.sojson.com`的静态资源都是拒绝空的`Referer` 的。如下图，我访问我的一个图片。

![img](https://cdn.yinshua86.com/file/16-01-23-02-57-10/doc/3910657428.jpg)





看到了吧，直接拒绝访问了，如果有同学在测试我网站的静态资源的时候，记住强制刷新`Ctrl + F5` ，因为浏览器有缓存，可能你开始还是可以访问的。

当然。这个你不能完全依赖[ Referer ](https://www.sojson.com/tag_referer.html)来做一些事情，因为这个最容易伪造来源。