# 安装

## 安装 Composer

如果还没有安装 Composer，你可以按 [getcomposer.org](https://getcomposer.org/download/) 中的方法安装。在 Linux 和 Mac OS X 中可以运行如下命令：

    curl -sS https://getcomposer.org/installer | php
    mv composer.phar /usr/local/bin/composer

在 Windows 中，你需要下载并运行 [Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe)。

如果遇到任何问题或者想更深入地学习 Composer，请参考 [Composer 文档（英文）](https://getcomposer.org/doc/)，[Composer 中文](https://github.com/5-say/composer-doc-cn)。

如果你已经安装有 Composer 请确保使用的是最新版本，你可以用 `composer self-update` 命令更新 Composer 为最新版本。

## 安装 Deruv
使用中国镜像：

    composer config -g repo.packagist composer https://packagist.phpcomposer.com
    
包管理插件：

    composer global require "fxp/composer-asset-plugin:^1.3.1"
  
安装Deruv

    composer create-project wskm/deruv

接下来你需要配置web服务器来访问Deruv的前台、后台。

## 配置Apache

在 Apache 的 `httpd.conf` 文件或在一个虚拟主机配置文件中使用如下配置。注意，你应该将 `path/to/basic/web` 替换为实际的 `basic/web` 目录。

```
DocumentRoot "path/to/basic/web"   # 设置文档根目录为 “basic/web”

<Directory "path/to/basic/web">
    # 开启 mod_rewrite 用于美化 URL 功能的支持（译注：对应 pretty URL 选项）
    RewriteEngine on
    # 如果请求的是真实存在的文件或目录，直接访问
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    # 如果请求的不是真实文件或目录，分发请求至 index.php
    RewriteRule . index.php

    # ...其它设置...
</Directory>
```

## 配置Nginx

为了使用 [Nginx](http://wiki.nginx.org/)，你应该已经将 PHP 安装为 [FPM SAPI](http://php.net/install.fpm) 了。使用如下 Nginx 配置，将 `path/to/basic/web` 替换为实际的 `basic/web` 目录，`mysite.local` 替换为实际的主机名以提供服务。

```
server {
    charset utf-8;
    client_max_body_size 128M;

    listen 80; ## 监听 ipv4 上的 80 端口
    #listen [::]:80 default_server ipv6only=on; ## 监听 ipv6 上的 80 端口

    server_name mysite.local;
    root        /path/to/basic/web;
    index       index.php;

    access_log  /path/to/basic/log/access.log main;
    error_log   /path/to/basic/log/error.log;

    location / {
        # 如果找不到真实存在的文件，把请求分发至 index.php
        try_files $uri $uri/ /index.php?$args;
    }

    # 若取消下面这段的注释，可避免 Yii 接管不存在文件的处理过程（404）
    #location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
    #    try_files $uri =404;
    #}
    #error_page 404 /404.html;

    location ~ \.php$ {
        include fastcgi.conf;
        fastcgi_pass   127.0.0.1:9000;
        #fastcgi_pass unix:/var/run/php5-fpm.sock;
        try_files $uri =404;
    }

    location ~ /\.(ht|svn|git) {
        deny all;
    }
}
```

注意当运行一个 HTTPS 服务器时，需要添加 `fastcgi_param HTTPS on;` 一行，这样 Yii 才能正确地判断连接是否安全。

## 使用前台的安装向导

你可以打开配置的前台地址，系统会自动跳转到安装页面。

>    运行根目录下的 init 文件，可以让系统相应目录的权限初始化为777，以便顺利安装。

