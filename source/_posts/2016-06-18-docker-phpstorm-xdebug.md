title: How to use xdebug with docker and phpstorm
date: 2016-06-18 10:55:10
categories:
- programming
tags:
- docker
- phpstore
- xdebug
- php
---

延續這篇文章"[網頁開發者如何使用Docker](http://lighter.github.io/2016/05/11/from-vagrant-to-docker-how-to-use-docker-for-local-web-development/)"建立完環境之後，在開發上為了trace執行的過程，因此想要加上xdebug。

首先修改原本`./php/Dockerfile`

```
FROM php:5.6-fpm
RUN docker-php-ext-install pdo_mysql

# 加入這些設定到xdebug.ini
RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini
RUN echo "xdebug.remote_connect_back=on" >> /usr/local/etc/php/conf.d/xdebug.ini
```

因為remote_hosts的ip可能不一定相通，所以將這個設定寫在`docker-compose.yml`。

```
php:
  build: ./php/
  expose:
    - 9000
  links:
    - mysql
  volumes_from:
    - app
  environment:
    XDEBUG_CONFIG: "remote_host=10.10.1.233 idekey=gtiapidocker remote_enable=1 remote_handler=dbgp remote_port=9000 remote_autostart=1 remote_connect_back=0 remote_log=/var/www/html/xdebug.log"
    PHP_IDE_CONFIG: "serverName=docker-ci"
```

執行下面指令取得目前您電腦連線的 IP，並指定給 remote_host

```
$ ipconfig getifaddr en0
```

以及注意到`PHP_IDE_CONFIG`的`serverName`，需與 PHPStorm Name 一致。

接著來設定PHPStorm。叫出Preferences(`command + ,`)，`Languages & Frameworks > PHP > Servers`，設定如下。

![](/images/docker_phpstorm_xdebug/docker-phpstorm-xdebug2.jpg)

接著點擊右上方，新增一個`PHP Remote Debug`

![](/images/docker_phpstorm_xdebug/docker-phpstorm-xdebug7.png)

`Servers`，選到剛剛新增的 `docker-ci`，`Ide key` 記得要和 `docker-compose.yml` 裡的設定一樣

![](/images/docker_phpstorm_xdebug/docker-phpstorm-xdebug5.png)

最後選到 `docker-ci`，並把話筒點開！監聽 xdebug。

![](/images/docker_phpstorm_xdebug/docker-phpstorm-xdebug6.png)

