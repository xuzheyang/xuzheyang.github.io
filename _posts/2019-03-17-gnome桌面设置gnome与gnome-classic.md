---
layout: post
title: "gnome桌面配置gnome与gnome-classic"
description: ""
categories: [Linux]
tags: [Linux, Gnome]
redirect_from:
  - /2019/03/17/
---


下面是一些“配置用户默认会话”官方文档:  
[Gnome](https://help.gnome.org/admin/system-admin-guide/stable/session-user.html.en)  
[Arch](https://wiki.archlinux.org/index.php/GNOME_(简体中文))  

但是这些方式对`Gnome Version 3.22.2`失效，进过一番实验之后发现一个可用方法：  

> $ cp /usr/share/xsessions/gnome-classic.desktop /usr/share/xsessions/gnome-classic.desktop.backup  
> $ vim /usr/share/xsessions/gnome-classic.desktop  
修改如下：  
~~~
#Exec=env GNOME_SHELL_SESSION_MODE=classic gnome-session --session gnome-classic
Exec=gnome-session
~~~

记录对于官房文档的疑惑：  
在修改`/var/lib/AccountsService/users/`路径下的文件后，重启计算机，文件都会恢复原来的文件，导致配置失效，个人猜测这些文件是开机后自动重新生成的。但是官方文档中描述的又不是这样，无法理解。
