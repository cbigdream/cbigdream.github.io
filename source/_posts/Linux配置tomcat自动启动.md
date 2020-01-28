---
title: Linux配置tomcat自动启动
date: 2019-05-22 21:15:00
categories: 技术
cover: https://i.loli.net/2020/01/28/LsQX7fMbogHA8lG.jpg
tags: 
    - linux
    - tomcat
---
将tomcat配置成linux service，跟随linux启动
<!-- more --> 
## 1. 编辑tomcat/bin/catalina.sh
在catalina.sh中添加java和tomcat环境变量
```shell
CATALINA_HOME=/usr/local/tomcat
JAVA_HOME=/usr/lib/java/jdk1.7.0_79
```
根据实际情况配置环境变量，然后将catalina.sh复制到/etc/init.d/目录下
```shell
cp tomcat/bin/catalina.sh /etc/init.d/tomcat
```
## 2. 更新tomcat service，并启动：
```shell
update-rc.d tomcat defaults
service tomcat start
```