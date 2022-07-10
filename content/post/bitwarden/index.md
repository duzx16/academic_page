---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "用Vaultwarden搭建自己的密码管理服务"
subtitle: ""
summary: Vaultwarden tutorial
authors: 
- Zhengxiao Du
tags: []
categories: [技术]
date: 2022-07-10T23:35:00+08:00
lastmod: 2022-07-10T23:35:00+08:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---
## 前言
密码是我们在互联网上使用各种服务的时候很难绕过的一环。我们经常能听到各种各样的科普文章中告诫我们，为了保护好自己的互联网账号，密码一定要足够复杂，最好能把大小写字母、数字、特殊符号都用上。但是，复杂的密码意味着难以记忆，而当下一个普通的互联网用户很可能拥有几十个账户。为了不折磨自己的脑细胞，很多人选择给多个账户使用同一个密码。然而，由于有的网站在数据安全方面疏于防护，用户的密码遭到泄漏的事故时有发生。虽然这些密码遭到泄漏的账户可能没有很高的价值，但是却可能导致使用了同样密码的高价值账户被盗。此外，有的人还在创建复杂密码的时候还会使用自己的姓名缩写、生日、手机号等信息，这也为通过社会工程学手段破解密码提供了可能。

为了解决这一问题，密码管理服务应运而生。密码管理服务为用户提供了存储互联网账户的用户名和密码的服务，往往还会和浏览器、手机操作系统等结合，提供自动填充、自动记录等功能。在安全性方面，无论是存储在本地还是云端，都会用用户设置的密码进行加密。有了密码管理服务，我们就可以给每个互联网账户使用随机生成的强密码。最常见的密码管理服务就是Chrome、Edge等浏览器自带的密码自动填充功能和iOS、MacOS提供的系统级密码管理。我之前使用的也是Chrome的自动填充功能，但是这个功能毕竟是绑定了Chrome浏览器和谷歌账号。这就限制了浏览器的选择，比如前段时间使用Chromium内核的Edge很火，但是因为自己的互联网账号密码都保存在Chrome上，所以无法尝试。类似的，苹果的密码管理服务也不方便在Windows和Android下使用。因此我最近决定迁移到相对独立的密码管理软件上。

## 为什么选择BitWarden
密码管理服务的安全性来自于现代加密算法，在无法攻破加密密码的情况下是无法获得加密内容的。但是为了能够在不同的设备上使用保管的密码，我们必须依赖于云端服务。而一旦涉及到云服务，就存在着很大的不确定性，因为我们很难检查服务器上实际运行的代码。这也是我没有选择很热门的1Password的原因。

BitWarden是前后端均开源的密码管理服务器，并且还支持由用户自己搭建后端服务。当然实际中大家使用更多的是[Vaultwarden](https://github.com/dani-garcia/vaultwarden)（原名为BitWarden_RS）这个非官方简化版后端实现，因为它占用的资源更少。同时BitWarden的前端也有很好的跨平台支持，对主流的浏览器和操作系统都提供了支持。

## 如何搭建自己的Vaultwarden服务
首先用docker容器运行Vaultwarden服务
```bash
docker pull vaultwarden/server:latest
mkdir /vw-data # this is used for permanent data storage
docker run -d --name vaultwarden \
 -e WEBSOCKET_ENABLED=true \
 -v /vw-data/:/data/ \
 -p 10020:80 -p 3012:3012 \
 vaultwarden/server:latest
```
其中10020和3012分别是主服务和WebSocket服务的端口，`/vw-data`是用来保存数据的文件夹。

然后在域名服务商的管理页面添加DNS记录，将准备的域名指向服务器的IP地址。作为例子，这里假设我们准备的域名是www.example.com，服务器的IP是1.1.1.1。

然后安装certbot
```bash
apt-get install -y snapd
snap install core; sudo snap refresh core
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbo
```

然后使用certbot进行证书生成
```bash
wget https://github.com/joohoi/acme-dns-certbot-joohoi/raw/master/acme-dns-auth.py
chmod +x acme-dns-auth.py # 注意要更改acme-dns-auth.py的首行的python为python3
mv acme-dns-auth.py /etc/letsencrypt/
certbot certonly --manual --manual-auth-hook /etc/letsencrypt/acme-dns-auth.py --preferred-challenges dns --debug-challenges -d \*.www.example.com -d www.example.com
```
在最后一步会要求你在域名的DNS配置中添加一个CNAME项来认证你的所有权，完成之后等待DNS更新之后按下回车。如果还没有更新完成就回车的话认证会失败，可以重新运行这一命令。证书相关的文件保存在`/etc/letsencrypt/live/www.example.com`下面。

最后在`/etc/nginx/conf.d/vaultwarden.conf`中进行nginx配置（需要先安装nginx）
```
# Redirect HTTP to HTTPS
server {
    listen 80;
    listen [::]:80;
    server_name www.example.com;
    return 301 https://$host$request_uri;
}

server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  server_name www.example.com;

  ssl_certificate /etc/letsencrypt/live/www.example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/www.example.com/privkey.pem;

  # Specify SSL config if using a shared one.
  #include conf.d/ssl/ssl.conf;

  # Allow large attachments
  client_max_body_size 128M;

  location / {
    proxy_pass http://127.0.0.1:10020;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }

  location /notifications/hub {
    proxy_pass http://127.0.0.1:3012;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
   }

  location /notifications/hub/negotiate {
    proxy_pass http://127.0.0.1:10020;
  }
}
```
然后就可以在www.example.com中访问自己的BitWarden服务了。在使用各个平台的BitWarden插件和客户端的时候，要在登陆之前先进行设置，将服务地址修改为www.example.com。
## 更新Vaultwarden镜像
```bash
# Pull the latest version
docker pull vaultwarden/server:latest

# Stop and remove the old container
docker stop vaultwarden
docker rm vaultwarden

# Start new container with the data mounted
docker run -d --name vaultwarden \
 -e WEBSOCKET_ENABLED=true \
 -v /vw-data/:/data/ \
 -p 10020:80 -p 3012:3012 \
 vaultwarden/server:latest
```
## 参考文献
> 1. Installing Vaultwarden formally bitwarden_rs on Ubuntu 20.04 with Nginx. https://www.llewellynhughes.co.uk/post/installing-vaultwarden
> 2. Vaultwarden Wiki. https://github.com/dani-garcia/vaultwarden/wiki
