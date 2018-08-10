---
layout: post
title: "Linux学习之CentOS利用devtools-set安装高版本gcc"
description: ""
categories: [Linux]
tags: [Linux , CentOS]
redirect_from:
  - /2017/10/02/
---

1. 获取root权限  

> $ su root

2. 安装`centos-release-sc`源  

> $ yum install centos-release-scl

3. 使用`centos-release-sc`源  

> $ yum-config-manager --enable rhel-server-rhscl-7-rpms  

4. 安装`devtoolset-6`  

> $ yum install devtoolset-6  

5. 在当前`bash`上应用  

> $ scl enable devtoolset-6 bash