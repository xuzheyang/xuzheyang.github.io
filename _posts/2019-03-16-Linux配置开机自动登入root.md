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

> $ sudo nano /usr/share/lightdm/lightdm.conf.d/60-lightdm-gtk-greeter.conf  

添加下面内容：  

~~~
[SeatDefaults]
greeter-session=lightdm-gtk-greeter 
autologin-user=root
~~~
