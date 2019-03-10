---
layout: post
title: "VMwareFusion从USB磁盘中启动"
description: ""
categories: [MacOS]
tags: [MacOS,VMware]
redirect_from:
  - /2019/03/10/
---

有时候我们有一个已经安装了操作系统的硬盘，但是身边没有空闲的电脑，那么怎么才能用虚拟机启动硬盘里的操作系统呢？首先你必须有一个`USB`接口的的外接硬盘盒。  

下述方式在`MacOS Mojave`与`VMware Fusion`专业版11.0.2,不一定在所有版本上都能用。  

1.查看磁盘  
> $ diskutil list  

下面是我的磁盘信息:  
![file](https://github.com/xuzheyang/xuzheyang.github.io/raw/master/_pic/2019-03-11/diskutil_list.png)  

2.查看分区编号  
> $ /Applications/VMware\ Fusion.app/Contents/Library/vmware-rawdiskCreator print /dev/disk2  

下面是我磁盘信息的分区编号:  
~~~
Nr      Start       Size Type Id Sytem                   
-- ---------- ---------- ---- -- ------------------------
 1       2048  468856832 BIOS  7 HPFS/NTFS
~~~ 

3.然后创建`VMDK`文件  
> $ /Applications/VMware\ Fusion.app/Contents/Library/vmware-rawdiskCreator create /dev/disk2 1 usb ide  

这个时候你会在当前文件夹下得到一个文件`usb-hdd.vmdk`  

4.用`VMware Fusion`虚拟机创建一个你需要的虚拟机并且把上述文件移动到你创建的虚拟机文件夹中  
> $ mv usb-hdd.vmdk Windows\ 10.vmwarevm  

5.打开虚拟机，删除所有磁盘与光驱设备  

6.打开虚拟机文件进行编译  

> $ vim Windows\ 10.vmx  

添加下面两句:  
~~~
ide1:0.present="TRUE"
ide1:0.fileName="usb-hdd.vmdk"  // 后面与你文件的相对路径对等
~~~

然后开机就行了  

