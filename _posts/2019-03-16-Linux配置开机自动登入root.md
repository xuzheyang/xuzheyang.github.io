---
layout: post
title: "Linux配置开机自动登入root"
description: ""
categories: [Linux]
tags: [Linux]
redirect_from:
  - /2019/03/16/
---

`Linux`开机自动登录与`Linux`桌面环境息息相关，每种桌面环境不一定一样  

# `Gnome`桌面  

> $ vim /etc/gdm/custom.conf  

添加下面内容：  

~~~
[daemon]
AutomaticLoginEnable=True
AutomaticLogin=root
~~~


# `Mate`桌面  

> $ vim  /etc/lightdm/lightdm.conf 

修改下面内容：  

~~~
autologin-user=root
~~~
