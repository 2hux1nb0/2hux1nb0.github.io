---
layout: post
title:  "为Debian搞定Mercury MW150US无线网卡驱动"
categories: [实施工程]
tags: [Linux]
---

#### 为Debian搞定Mercury MW150US无线网卡驱动

>买无线网卡前没认真研究Linux兼容性的问题，看着水星的mini无线网卡小巧便捷就买回来了。狗哥了一下才发现Linux不支持这款，得自己搞定驱动。  

### PS: 买无线网卡的时候一定要看是否支持Linux，需要用到虚拟机的看是否支持虚拟机，或者买了自己去配置鼓捣吧

先安装些编译工具:  

```c
apt-get install build-essential linux-headers make gcc git autoconf
```

下载驱动程序源码：  

```c#
git clone --depth=1 https://github.com/gleb-chipiga/rtl8188eu

cd rtl8188eu && make
```

安装驱动：

```php
cp 8188eu.ko /lib/modules/`uname -r`/kernel/drivers/net/wireless

echo "8188eu" >> /etc/modules

vi /etc/network/interface
```

加入以下命令：

```java
auto wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp
    wpa-ssid TP_LINK_XXXX
    wpa-psk XXXXXXXXXX
```
