---
layout: post
title: "Linux学习之CentOS修复grub2引导Windows"
description: ""
categories: [Linux]
tags: [Linux , centos , grub2 , 引导, Windows]
redirect_from:
  - /2018/01/02/
---

# 目录  

> * [I、安装ntfs-3g](#one)  
> * [II、修复引导](#two)  


<a name="one"></a>

# I、安装ntfs-3g  

由于`Linux`系统不识别`Windows`的`ntfs`分区，所以需要安装一下小插件  

1.添加源  

> $ wget -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo  

2.安装  

> $ yum install ntfs-3g  


<a name="two"></a>

# II、修复引导  

> $ grub2-mkconfig -o /boot/grub2/grub.cfg  
