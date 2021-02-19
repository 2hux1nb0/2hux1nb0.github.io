---
layout: post
title:  "Windows查看当前电脑连接过的所有WiFi密码"
categories: [网络安全CyberSecurity]
tags: [WiFi]
---

## Windows命令行查看当前电脑连接过的所有WIFI密码

```powershell
for /f "skip=9 tokens=1,2 delims=:" %i in ('netsh wlan show profiles') do  @echo %j | findstr -i -v echo | netsh wlan show profiles %j key=clear
```
