### docker部署php项目

#### dockerfile
```
FROM php:7.2-fpm-alpine3.12
COPY . /usr/src/myapp
WORKDIR /usr/src/myapp

RUN apk update; \
    apk add nginx; \
    cd /run;\
    mkdir nginx;\
    touch nginx.pid;\
    cd /usr/src/myapp;\
    curl -sS https://getcomposer.org/installer | php; \
    mv /usr/src/myapp/composer.phar /usr/local/bin/composer; \
    composer update //安装composer
CMD ["nginx", "-g", "daemon off;"] //启动nginx

```
