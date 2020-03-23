<!-- 
 - @Author:		徐哲阳
 - @EMail:		xuzheyangchn@foxmail.com
 - @FileName:	2020-03-18-Linux搭建DHCP服务器.md
 - @Created:	2020-03-18 21:55:45
 - @Description: 
 -->

---
layout: post
title: "Linux搭建DHCP服务器"
description: ""
categories: [Linux]
tags: [Linux , DHCP]
redirect_from:
  - /2020/03/18/
---

# 目录  

> * [I、关于DHCP服务](#one)  
> * [II、安装DHCP服务](#two)  
> * [III、配置DHCP服务器](#three)  
> * [IV、开启并用客户端连接](#four)  


<a name="one"></a>

# I、关于DHCP服务

DHCP（动态主机配置协议）是一个局域网的网络协议。指的是由服务器控制一段lP地址范围，客户机登录服务器时就可以自动获得服务器分配的lP地址和子网掩码。


<a name="two"></a>

# II、安装DHCP服务  

1.Fedora  
> $ dnf install dhcp  

2.Centos/RHEL  
> $ yum install dhcp  

3.ubuntu/debian  
> $ apt-get install dhcp  


<a name="three"></a>

# III、配置DHCP服务器  

## 文件列表  

~~~
 - /etc/dhcp/dhcpd.conf           # DHCP的配置文件
|- /usr/sbin/dhcpd                # 二进制服务程序
 - /var/lib/dhcp/dhcpd.leases     # 租赁记录
~~~

## 配置  

> $ vim /etc/dhcp/dhcpd.conf

~~~
# 1. 整体的环境设定
ddns-update-style            none;            #<==不要更新 DDNS 的设定
ignore client-updates;                        #<==忽略客户端的 DNS 更新功能
default-lease-time           259200;          #<==预设租约为 3 天
max-lease-time               518400;          #<==最大租约为 6 天
option routers               192.168.100.254; #<==这就是预设路由
option domain-name           "centos.vbird";  #<==给予一个领域名
option domain-name-servers   168.95.1.1, 139.175.10.20;
# 上面是 DNS 的 IP 设定，这个设定值会修改客户端的 /etc/resolv.conf 档案内容

# 2. 关于动态分配的 IP
subnet 192.168.100.0 netmask 255.255.255.0 {
    range 192.168.100.101 192.168.100.200;  #<==分配的 IP 范围

    # 3. 关于固定的 IP 啊！
    host win7 {
        hardware ethernet    08:00:27:11:EB:C2; #<==客户端网卡 MAC
        fixed-address        192.168.100.30;    #<==给予固定的 IP
    }
}
# 相关的设定参数意义，请查询前一小节的介绍，或者 man dhcpd.conf
~~~

> $ vim /etc/sysconfig/dhcpd

~~~
DHCPDARGS="eth0"    # 监控的网卡
~~~


<a name="four"></a>

# IV、开启并用客户端连接 

> $ systemctl start dhcpd  
> $ systemctl enable dhcpd  
