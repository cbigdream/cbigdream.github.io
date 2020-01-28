---
title: 使用caddy作为HTTPS服务器
date: 2019-05-22 16:18:06
categories: 技术
cover: https://i.loli.net/2020/01/28/7zdcqnKFT5CRjku.jpg
tags: [linux,caddy,https]
---
用Caddy的插件forwardproxy可以快速搭建HTTPS服务器，可以不用手动配置ssl证书
<!-- more --> 
## 1. 安装Caddy和Caddy插件forwardproxy
```
curl https://getcaddy.com | bash -s personal http.forwardproxy
```
已经安装过Caddy的也可以使用上述命令，升级Caddy（如果有新版本），并安装插件forwardproxy，随后重启Caddy即可。
```
service caddy restart
OR
systemctl restart caddy.service
```
## 2. 配置caddy
### 1. 创建配置文件放到 /etc/caddy 目录
```
sudo mkdir /etc/caddy
sudo touch /etc/caddy/Caddyfile
sudo chown -R root:www-data /etc/caddy
```
### 2. 配置ssl证书目录
```
sudo mkdir /etc/ssl/caddy
sudo chown -R www-data:root /etc/ssl/caddy
sudo chmod 0770 /etc/ssl/caddy
```
### 3. 配置 systemd
```
sudo curl -s  https://raw.githubusercontent.com/mholt/caddy/master/dist/init/linux-systemd/caddy.service  -o /etc/systemd/system/caddy.service
sudo systemctl daemon-reload
sudo systemctl enable caddy.service
sudo systemctl status caddy.service
```
### 4. 配置Caddfile配置文件
```
域名 {
    root /data/wwwroot/default
    tls justforjoy7@gmail.com
    log stdout 
    forwardproxy {
        ports 80 443
        #basicauth user pass
        hide_ip
        hide_via
    }
}
```
为了掩饰站点，可以在例如/data/wwwroot/default网站目录下放些东西。
### 5. 通过systemd管理caddy
```
sudo systemctl start caddy.service
sudo systemctl stop caddy.service
sudo systemctl restart caddy.service
sudo systemctl reload caddy.service
```