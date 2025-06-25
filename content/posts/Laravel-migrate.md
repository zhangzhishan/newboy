---
title: Laravel 项目移植指南
tags: Laravel, PHP
date: 20 Nov 2016 11:34 PM
---
由于一些特殊原因，虽然采用 Laravel 开发的网站，但是并没有使用 Vagrant，于是只得手工移植。下面是具体流程。
## 代码移植
采用的 Github 进行版本控制及管理，所以很简单，单纯的配置好 Github，然后`git pull` 一下。
## 安装composer
当然，作为一个现代的 PHP 开发，我们采用 composer 进行依赖管理，具体安装方法可以参考[官网](https://getcomposer.org/download/), 基本如下：

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '92102166af5abdb03f49ce52a40591073a7b859a86e8ff13338cf7db58a19f7844fbc0bb79b2773bf30791e935dbd938') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

上面是全局安装，将 composer 安装到系统环境变量 PATH 所包含的路径下面，然后就能够在命令行窗口中直接执行 composer 命令了。
但是由于一些众所周知的原因，composer 在国内无法访问，只得采用[国内的源](http://pkg.phpcomposer.com/#how-to-install-composer)，所以我们需要修改 composer 的全局配置文件

```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```
一切就绪后就可以执行

```
  composer install
```
由于网络环境的问题，可能会出现各种 time out， 多试几次就好了。


## 修改 PHP 配置文件
这时候如果试着用`php artisan serve`运行服务器，可能会出现类似下面的错误。
```[Symfony\Component\Process\Exception\RuntimeException]
  The Process class relies on proc_open, which is not available on your PHP installation.
```
这是 PHP 配置文件的问题，可以通过修改`php.ini`中的`disable_functions`，删除掉报错的相关函数即刻。 
## 路由 rewrite
假设已经配置好基本的网站服务器文件，这时候还需要进行路由重写，这个主要是使得链接更加美观。

### Nginx 配置
```
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```
### Apache 配置
首先确保打开`mod_rewrite`模块，然后修改`.htaccess`如下

```
Options +FollowSymLinks
RewriteEngine On

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [L]
```
>[呼呼呼山](http://code4fun.me)

