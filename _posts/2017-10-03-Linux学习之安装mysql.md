---
layout: post
title: "Linux学习之安装mysql"
description: ""
categories: [Linux]
tags: [Linux , mysql]
redirect_from:
  - /2017/10/03/
---

### 安装`MySQL`的`yum`源  

1.Fedora 27/26/25  

> ## Fedora 27  
> $ dnf install https://repo.mysql.com/mysql57-community-release-fc27-10.noarch.rpm
> ## Fedora 26  
> $ dnf install https://repo.mysql.com/mysql57-community-release-fc26-10.noarch.rpm  
> ## Fedora 25  
> $ dnf install https://repo.mysql.com/mysql57-community-release-fc25-10.noarch.rpm    

2.CentOS and Red Hat (RHEL)  

> ## CentOS 7 and Red Hat (RHEL) 7  
> $ yum localinstall https://repo.mysql.com/mysql57-community-release-el7-11.noarch.rpm  
> ## CentOS 6 and Red Hat (RHEL) 6  
> $ yum localinstall https://repo.mysql.com/mysql57-community-release-el6-11.noarch.rpm  

### 安装`mysql`  

1.Fedora 26/25/24  

> $ dnf install mysql-community-server  

2.CentOS 7.4/6.9 and Red Hat (RHEL) 7.4/6.9   

> $ yum install mysql-community-server  


### 启动`mysql`和设置开机自启动   

1.Fedora 26/25/24 and CentOS 7.4 and Red Hat (RHEL) 7.4  

> $ systemctl start mysqld.service  
> $ systemctl enable mysqld.service    

2.CentOS 6.9 and Red Hat (RHEL) 6.9  

> $ service mysql start  
> $ chkconfig --levels 235 mysqld on  

### 获取`mysql`的初始密码  

> $ grep 'A temporary password is generated for root@localhost' /var/log/mysqld.log |tail -1  

~~~
Example Output:
2015-11-20T21:11:44.229891Z 1 [Note] A temporary password is generated for root@localhost: -et)QoL4MLid
And root password is: -et)QoL4MLid
~~~


### 修改`mysql`的`root`密码  

> $ /usr/bin/mysql_secure_installation  

### 打开`mysql`远程访问权限  

> $ mysql -uroot -p  
> $ mysql> GRANT ALL PRIVILEGES ON \*.\* TO 'root'@'%'IDENTIFIED BY 'mypassword' WITH GRANT OPTION;   
> $ mysql> FLUSH PRIVILEGES;  

### 设置防火墙  

1.Fedora 26/25/24 and CentOS/Red Hat (RHEL) 7.4  

> $ firewall-cmd \-\-permanent \-\-zone=public \-\-add-service=mysql  
> $ systemctl restart firewalld.service  

2.CentOS/Red Hat (RHEL) 6.9  

> $ nano -w /etc/sysconfig/iptables  
> $ -A INPUT -m state \-\-state NEW -m tcp -p tcp \-\-dport 3306 -j ACCEPT  
> $ service iptables restart  
