---
title: macOS PHP 开发环境配置
tags: macOS
categories: PHP
date: 2016-05-29 00:51:49
---
macOS 上的 homebrew 和 ubuntu 上的 apt-get 这些包管理器的最大的好处，就是将我们从无休止的编译中解放出来，让环境配置变得如此容易。不用去管这条、那条的编译条件。
下面是基于 homebrew 的安装配置流程。
## 准备工作
- 联网
- 安装 XCode-select（用来安装homebrew）
- 安装 homebrew
## 安装最新版 PHP
根据[鸟哥的博客](http://www.lanecn.com/article/main/aid-97)，PHP 7 有了很大的性能提升，而程序用的框架 Laravel 又及时地支持了，用 homebrew 安装代码如下：

```
brew tap homebrew/dupes
brew tap homebrew/versions
brew tap homebrew/homebrew-php
brew install php70
```
安装好后需要修改一下配置文件`/usr/local/etc/php/7.0/php.ini`
在PHP.INI中设置时区
```
date.timezone = PRC或date.timezone = Asia/Shanghai
```
## 安装 nginx

```
brew install nginx
```
### nginx 的配置

```
vi /usr/local/etc/nginx/nginx.conf
vi /usr/local/etc/nginx/vhost/example.com.conf
```
### vhost 配置

```
server {
listen 8888;
server_name localhost ;
access_log /Users/zhishanzhang/Sites/wwwlogs/example.com_nginx.log combined;
index index.html index.htm index.php;
root /Users/zhishanzhang/Sites/pdh;
location ~ .*\.(wma|wmv|asf|mp3|mmf|zip|rar|jpg|gif|png|swf|flv)$ {
    valid_referers none blocked *.localhost localhost ;
    if ($invalid_referer) {
        return 403;
        }
    }
location ~ [^/]\.php(/|$) {
    fastcgi_pass 127.0.0.1:9000;
#    fastcgi_pass unix:/dev/shm/php-cgi.sock;
    fastcgi_index index.php;
    include fastcgi.conf;
    }
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|ico)$ {
    expires 30d;
    access_log off;
    }
location ~ .*\.(js|css)?$ {
    expires 7d;
    access_log off;
    }
}
```
### 一些命令：

Nginx web server.

- Start server with default config:

    nginx

- Start server with custom config file:

    nginx -c config_file

- Start server with a prefix for all relative paths in config file:

    nginx -c config_file -p prefix/for/relative/paths

- Test configuration without affecting the running server:

    nginx -t

- Reload configuration by sending a signal with no downtime:

    nginx -s reload



## 配置 php-fpm

`vi /usr/local/etc/php/7.0/php-fpm.d/www.conf`

```
listen = 127.0.0.1:9000
```
## 安装 xdebug

```
brew install php70-xdebug
vi /usr/local/etc/php/7.0/conf.d/ext-xdebug.ini
```

```
[xdebug]
zend_extension="/usr/local/opt/php70-xdebug/xdebug.so"
xdebug.remote_enable=1
xdebug.remote_port=10000
xdebug.profile_enable=1
```
## FAQ
- 出现 502 Bad Gateway 是什么原因？

一般应该是 php-fpm 的原因，检查 nginx 配置中 `fastcgi_pass` 的是采用的端口还是`sock`，这个要和`php-fpm`的配置中保持一样

- 出现 403 Forbidden 的原因

这个肯定是权限问题，检查文件夹的权限，我出现问题的原因是，由于用的是之前的配置环境，里面有对于资源引用的一个判断，抛出 403 错误

- 出现首页可以打开，但是点击别的链接 404 Not Found 的原因

这个一般就是，域名的重写问题，如果是 Apache 可以检查 `.htacess` 文件，如果是 nginx 可以检查看配置中是否有重写的规则，例如下面就是 Laravel 的域名重写规则。

```
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```

>[呼呼呼山](http://code4fun.me)

