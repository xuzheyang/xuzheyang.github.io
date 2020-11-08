---
layout: post
title: "Fedora33破解navicat15"
description: ""
categories: [Linux]
tags: [Linux , Navicat15]
redirect_from:
  - /2020/11/08/
---

# 目录  

> * [I、下载所需要的文件](#one)  
> * [II、安装编译依赖](#two)  
> * [III、编译 keystone](#three)  
> * [IV、编译 patcher-keygen](#four)  
> * [V、编译 patch Navicat](#five)  
> * [VI、制作 AppImage](#six)  
> * [VII、复制到系统](#seven)  
> * [VIII、注册 Navicat](#eight)  


<a name="one"></a>

# I、下载所需要的文件  

> $ mkdir ~/navicat15  
> $ cd ~/navicat15  
> $ git clone https://gitee.com/xuzheyang/keystone.git 
> $ git clone https://gitee.com/xuzheyang/navicat-keygen.git  
> $ wget https://github.com/AppImage/AppImageKit/releases/download/continuous/appimagetool-x86_64.AppImage  
> $ wget http://download.navicat.com.cn/download/navicat15-premium-cs.AppImage  


<a name="two"></a>

# II、安装编译依赖  

1.安装 capstone  
> $ sudo dnf install capstone-devel  
> or
> $ sudo apt-get install libcapstone-dev  

2.安装 rapidjson  
> $ sudo dnf install rapidjson-devel  
> or
> $ sudo apt-get install rapidjson-dev  

3. 安装编译环境  
> $ sudo dnf install openssl-devel gcc gcc-c++ make cmake  
or 
> $ sudo apt-get install libssl-dev gcc g++ make cmake  


<a name="three"></a>

# 编译 keystone  

> $ cd ~/navicat15/keystone  
> $ mkdir build && cd build && ../make-share.sh  
> $ sudo make install  
> $ sudo ln -sf /usr/local/lib64/libkeystone.so* /usr/lib64    


<a name="four"></a>

# IV、编译 patcher-keygen  

$ cd ~/navicat15/navicat-keygen  
$ make all  


<a name="five"></a>

# V、patch Navicat

> $ cd ~/navicat15  
> $ sudo mount -o loop ~/navicat15/navicat15-premium-cs.AppImage /mnt  
> $ rsync -a /mnt/ ~/navicat15/navicat15-premium-cs-patched  
> $ sudo umount /mnt  
> $ ~/navicat15/navicat-keygen/bin/navicat-patcher ~/navicat15/navicat15-premium-cs-patched  


<a name="six"></a>

# VI、制作 AppImage  

> $ chmod +x ~/navicat15/appimagetool-x86_64.AppImage  
> $ ~/navicat15/appimagetool-x86_64.AppImage ~/navicat15/navicat15-premium-cs-patched ~/navicat15/navicat15-premium-cs-patched.AppImage  


<a name="seven"></a>

# VII、复制到系统  

> $ mkdir /opt/navicat15  
> $ cp ~/navicat15/navicat15-premium-cs-patched.AppImage /opt/navicat15  
> $ vim /opt/navicat15/navicat15-premium.desktop  
~~~
[Desktop Entry]
Comment=navicat15-premium
Comment[zh_CN]=navicat15-premium
Exec=/opt/navicat15/navicat15-premium-cs-patched.AppImage %U
Name=Navicat15 Premium
Name[zh_CN]=Navicat15 Premium
StartupNotify=false
Terminal=false
Type=Application
Icon=/opt/navicat15/Premium15.png
~~~

> $ cp ~/navicat15/navicat15-premium-cs-patched/navicat-icon.png /opt/navicat15/Premium15.png  

> ln -sf /opt/navicat15/navicat15-premium.desktop /usr/share/applications  


<a name="eight"></a>

# VIII、注册 Navicat  

> $ ~/navicat15/navicat-keygen/bin/navicat-keygen --text ~/navicat15/RegPrivateKey.pem  
~~~
输入 1. Premium 的序号，回车
输入 1. Simplified Chinese 的序号，回车
输入navicat15的主版本号，我这里是15
把Serial number下面的xxx-xxx-xxxx-xxxx拷贝到 Navicat 的序列号中激活
选择手动激活，然后把请求号拷贝到终端按两次回车
最后把激活码复制到navicat中
~~~
