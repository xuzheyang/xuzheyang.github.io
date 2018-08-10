---
layout: post
title: "Linux学习之CentOS7内核编译"
description: ""
categories: [Linux]
tags: [Linux , CentOS , 内核编程]
redirect_from:
  - /2017/02/14/
---

# 目录  

> * [I、编译前的准备](#one)  
> * [II、安装当前版本内核源码](#two)  
> * [III、编译内核](#three)  
> * [IV、安装内核](#four)  

注意:本文以3.10.0-514.el7.x86_64为例，记得改成自己的版本号  


<a name="one"></a>

# I、编译前的准备  

登录root用户 然后执行下面的步骤  
> $ yum install  

1.首先安装（升级）一些依赖包：
> $ yum install vim gcc gcc-c++ rpm-build redhat-rpm-config asciidoc  
> $ yum install hmaccalc perl-ExtUtils-Embed pesign xmlto audit-libs-devel  
> $ yum install binutils-devel elfutils-devel elfutils-libelf-devel  
> $ yum install ncurses-devel newt-devel numactl-devel pciutils-devel  
> $ yum install python-devel zlib-devel bison bison-devel

2.创建源码的编译目录树，目的源码存放地址  
> $ cd ~  
> $ mkdir -p ~/rpmbuild/{BUILD,BUILDROOT,RPMS,SOURCES,SPECS,SRPMS}  
> $ `echo '%_topdir %(echo $HOME)/rpmbuild' > ~/.rpmmacros`  


<a name="two"></a>

# II、安装当前版本内核源码  

1.找到你的centos对应的内核源码包

> $ uname -r  // 查看内核版本，如 3.10.0-514.el7.x86_64

2.然后从[`centos`官网](http://vault.centos.org/)上下载内核源码包

3.安装内核  
> $ rpm -i kernel-3.10.0-514.el7.x86_64.rpm 2>&1 | grep -v exist

这时文件就生成到`~/rpmbuild`目录下了


<a name="three"></a>

# III、编译内核

1.内核源码解压
> $ cd ~/rpmbuild/SPECS/  
> $ rpmbuild -bp kernel.spec  
> $ cd ~/rpmbuild/BUILD/kernel-3.10.0-514.el7  
> $ cp -aR linux-3.10.0-514.el7.centos.x86_64/ linux-3.10.0-514.el7.centos.x86_64.mine

2.在内核源码里修改你想要修改的地方后生成补丁文件  
> $ cd ~/rpmbuild/BUILD/kernel-3.10.0-514.el7  
> $ diff -urpN linux-3.10.0-514.el7.centos.x86_64 linux-3.10.0-514.el7.centos.x86_64.mine > linux-3.10.0-514.el7.centos.x86_64-20170205-V0.1.patch

3.备份你的`patch`文件，然后拷贝到下面的目录
> $ cp -a linux-3.10.0-514.el7.centos.x86_64-20170205-V0.1.patch ~/rpmbuild/SOURCES/

4.更改内核的`spec`文件
> $ cd ~/rpmbuild/SPECS  
> $ cp -a kernel.spec kernel.spec.distro  
> $ vim kernel.spec  

buildid 的定义本来是一个注释。它必须被取消注释及赋予一个数值，好避免与你现时安装了的内核互相抵触。这将这行更改如下列样子般：
~~~
%define buildid .xzy  // 自定义的后缀名

# xzy
Patch2000: linux-3.10.0-514.el7.centos.x86_64-20170205-V0.1.patch

# xzy
ApplyOptionalPatch linux-3.10.0-514.el7.centos.x86_64-20170205-V0.1.patch

%changelog
# xzy
* Sun Feb 05 2017 ZheYang Xu <xuzheyangchn@163.com> - 3.10.0-514.el7.centos
-- Change code
~~~

5.编译新内核
> $ rpmbuild -bb --target=\`uname -m\` kernel.spec 2> build-err.log | tee

在上面的rpmbuild过程中，会检查ABI，如果新旧ABI的值不一样，会报下面这种错误
~~~
		*** ERROR - ABI BREAKAGE WAS DETECTED ***

		The following symbols have been changed (this will cause an ABI breakage):

		    bus_register
		    bus_unregister
		    dev_get_drvdata
		    dev_set_drvdata

		    + exit 1
		    错误：/var/tmp/rpm-tmp.iZXJuR (%build) 退出状态不好


		    RPM 构建错误：
		        /var/tmp/rpm-tmp.iZXJuR (%build) 退出状态不好
~~~

如果没有错误，那新的内核的所有相关rpm都生成到目录 ~/rpmbuild/RPMS/x86_64 下了，把他们拷贝出来备份。


<a name="four"></a>

# IV、安装内核   

> // 使用rpm 安装新内核（我们测试内核功能可以使用--force 强制更换新内核）  
> $ cd ~/rpmbuild/RPMS/x86_64  
> $ rpm -ivh * --force  //安装完后重启即可
