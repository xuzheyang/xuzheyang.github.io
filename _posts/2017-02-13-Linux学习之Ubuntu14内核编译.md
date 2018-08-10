---
layout: post
title: "Linux学习之Ubuntu14内核编译"
description: ""
categories: [Linux]
tags: [Linux , ubuntu , 内核编程]
redirect_from:
  - /2017/02/13/
---

### 目录  

> * [I、下载内核源码](#one)
> * [II、配置内核选项](#two)
> * [III、编译内核](#three)
> * [IV、编译模块](#four)
> * [V、安装内核](#five)
> * [VI、修改grub](#six)

### 注意：本文以linux-source-4.8.0为例，记得改成自己的版本号  

### I、下载内核源码<a name="one"></a>  

一、查找内核：  

> $ sudo apt-get update  
> $ sudo apt-get dist-upgrade  // 如果不需要更新请跳过此命令  
> $ apt-cache search linux-source  

二、查看内核版本：  

> $ uname -r  

三、下载内核：  

内核源码的下载方式有很多：  

1. 直接进入官网下载源码，[官网下载](http://www.kernel.org/)  
2. 命令下载：  

> sudo apt-get install linux-source // 系统会自动适配当前合适的源码，上述步骤可以忽略  

注意：命令下载的内核路径为`/usr/src/linux-source-4.8.0`  

### II、配置内核选项：<a name="two"></a>  

一、解压内核：  

> // 进入目录解压
> $ cd /usr/src/linux-source-4.8.0  
> $ tar xvf linux-source-4.8.0.tar.bz2  // 把linux-source替换成内核源码包名
> $ cd linux-source-4.8.0  // 解压完成后进入文件夹  

注意：`/usr/src`文件夹中有一个`linux-source-4.8.0.tar.bz2`压缩的源码包是一个符号连接，不用管他  

二、配置内核：  

1.输入`cp /boot/config-`，然后按下`Tab`键，系统会自动填上该目录下符合条件的文件名，然后继续输入`.config`，目的是使用在`boot`目录下的原配置文件。如果`/usr/src`下有`.config`文件，也不要使用，因为`/boot/`下的配置文件更新一些。

> $ cp /boot/config-4.8.0-37-generic .config  

2.清理代码树  
如果使用刚下载的完整的源程序包即第一次进行编译，那么本步可以省略。
`make mrproper`（如果你只是在原编译版本上修改了.config的少许选项，而希望他选项保留的话，不要执行这一步，否则你需要从头开始编译！！！）

作用是在每次配置并重新编译内核前需要先执行“make mrproper”命令清理源代码树，包括过去曾经配置的内核配置文件“.config”都将被清除。即进行新的编译工作时将原来老的配置文件给删除到，以免影响新的内核编译，即检查有无不正确的.o文件和依赖关系，如果使用刚下载的完整的源程序包即第一次进行编译，那么本步可以省略。而如果你多次使用了这些源程序编译内核，则最好要先运行一下这个命令。


3.配置内核

安装一个依赖，不然`make menuconfig`会报错  

> $ sudo apt-get install libncurses5-dev  
>	$ sudo make menuconfig     

Note：可能需要提示安装相应库，一般执行 apt-get install libncurses5-dev 即可；在配置中修改General Setup中的Local version - append to kernel release这项以添加一些个性化的内核名字，例如 -xzy-2017-02-04

4.安装依赖  
安装依赖，不然编译时候会报错  

> $ apt-get install libssl-dev

### III、编译内核：<a name="three"></a>  

5.编译内核（先不改内核吧，等编译通过后再添加io管控的内核代码）

> $	make -j4  

（用make -j带一个参数，可以把项目在进行并行编译，在多核CPU上，适当的进行并行编译还是可以明显提高编译速度的。但并行的任务不宜太多，一般是以CPU的核心数目的两倍为宜。 一般会生成大约2-3个小时吧），该命令会生成内核模块和vmlinuz，initrd.img，Symtem.map文件。

### IV、编译、安装模块：<a name="four"></a>  

1.编译模块：  

> $ sudo make modules  

2.安装模块：  

> $ make modules_install

### V、安装内核：<a name="five"></a>  

> $ make install

### VI、修改grub：<a name="six"></a>  
修改/etc/default/grub ，让grub菜单显示
	vim /etc/default/grub

	grub配置说明：

```
1.GRUB_HIDDEN_TIMEOUT=0
        此配置将影响菜单显示。若设置此选项,将在此时间内隐藏菜单而显示引导画面。
        菜单将会被隐藏,除非在此行开头加上一个 # 符号。(# GRUB_HIDDEN_TIMEOUT=0)。
        GRUB 2 第一次执行时将会寻找其他操作系统。若没有其他操作系统被检测到,菜单将会配置为隐藏。若辨认出其他操作系统,菜单将会显示。
        若是大于 0 的整数,系统将会依此配置的秒数暂停,但不会显示菜单。
         0 则菜单不会显示,也不会有延迟。
        使用者可以在启动时按住 SHIFT 键不放以强制显示菜单。
        启动过程中,系统将会检查 SHIFT 键状态。若无法辨识按键状态,会有一个短时间的延迟让使用者可通过按下 ESC 键来显示菜单。

2.GRUB_HIDDEN_TIMEOUT_QUIET=true
        true 不显示倒计时。屏幕将会是空白的。
        false 在 GRUB_HIDDEN_TIMEOUT 中配置的时间,空白屏幕上会有一个倒数计时器。

3.GRUB_TIMEOUT=10
        此命令将顺从 GRUB_HIDDEN_TIMEOUT 配置,除非 GRUB_HIDDEN_TIMEOUT 被注释掉(#)。若 GRUB_HIDDEN_TIMEOUT 启用,则当菜单显示时,GRUB_TIMEOUT 将会只执行一次。
        配置此值为 -1 将会导致菜单一直显示,直到用户选择。
        GRUB 2 菜单默认为隐藏,除非其他操作系统被系统检测到。若没有其他操作系统,此行将会被注释掉,除非使用者修改它。为了在每次启动时显示菜单,去掉此行的注释并使用 1 或更大的值。

```

修改前/etc/default/grub 6、7、8、9行的内容是：
```
GRUB_DEFAULT=0
GRUB_HIDDEN_TIMEOUT=0
GRUB_HIDDEN_TIMEOUT_QUIET=true
GRUB_TIMEOUT=10
```

 	修改后 /etc/default/grub 6、7、8、9行的内容是：
```
GRUB_DEFAULT=0
#GRUB_HIDDEN_TIMEOUT=0
GRUB_HIDDEN_TIMEOUT_QUIET=true
GRUB_TIMEOUT=10
```
	刷新grub.cfg
> $	update-grub2

### reboot
