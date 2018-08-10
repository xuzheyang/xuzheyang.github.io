---
layout: post
title: "Linux学习之Ubuntu启用root用户"
description: ""
categories: [Linux]
tags: [Linux , ubuntu]
redirect_from:
  - /2017/02/12/
---

# 目录  

> * [I、版本适用说明](#one)  
> * [II、更新系统及安装软件](#two)  
> * [III、开启root帐户登录](#three)  


<a name="one"></a>

# I、版本适用说明  

该文档适用于 Ubuntu 14、Ubuntu 16  


<a name="two"></a>

# II、更新系统及安装软件  

如果你并不想更新系统请跳过：  

> $ sudo apt-get update  
> $ sudo apt-get dist-upgrade  
> $ sudo apt-get install vim  


<a name="three"></a>

# III、开启root账户登录  

1. 修改root帐户密码  
> $ sudo passwd root  
2. 修改配置  
> $ vim /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf  
> // 在末行添加：  
> greeter-show-manual-login=true  
> 保存关闭
3. 重启登录以root帐户登录  
4. 如果有错误提示：（读取/root/.profile时发生错误: mesg.ttyname failed 对设备不适当的ioctl操作 作为结果， 会话不会被正确配置）

解决方法：  
将非root用户账户目录的.profile复制到/root  
		全新的文件.profile的内容大概是这样的：  

~~~  
# ~/.profile: executed by the command interpreter for login shells.
# This file is not read by bash(1), if ~/.bash_profile or ~/.bash_login
# exists.
# see /usr/share/doc/bash/examples/startup-files for examples.
# the files are located in the bash-doc package.

# the default umask is set in /etc/profile; for setting the umask
# for ssh logins, install and configure the libpam-umask package.
#umask 022

# if running bash
if [ -n "$BASH_VERSION" ]; then
    # include .bashrc if it exists
    if [ -f "$HOME/.bashrc" ]; then
        . "$HOME/.bashrc"
    fi
fi

# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH"
fi
~~~  

重启以root登录系统
