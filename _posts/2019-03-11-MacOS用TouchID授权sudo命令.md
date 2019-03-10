---
layout: post
title: "MacOS用TouchID授权sudo命令"
description: ""
categories: [MacOS]
tags: [MacOS]
redirect_from:
  - /2019/03/11/
---

无意中在网上看到这篇博客，感觉是个好东西 ，记录一下[原文地址](https://sspai.com/post/42038)  

# 系统环境  
由于系统的功能是随着升级而不断变化的，所以不保证长期有效。  
~~~
机型：MacBook Pro 15" 2017  
系统：macOS 10.14.4 Beta  
~~~

# 操作方法  
打开“终端”，执行以下命令:  
> $ sudo sed -i ".bak" '2s/^/auth sufficient pam_tid.so\'$'\n/g' /etc/pam.d/sudo  
