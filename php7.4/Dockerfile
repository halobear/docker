FROM php:7.4-fpm-alpine

ARG TZ=Asia/Shanghai
ARG PHP_EXTENSIONS=zip,bcmath,pdo_mysql,gd,opcache,xmlrpc,redis,pcntl,sockets,exif

COPY ./conf/. /usr/local/etc/php/
COPY ./extensions /tmp/extensions 

RUN sed -i "s/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g" /etc/apk/repositories \
    && apk update \
    && cd /tmp/extensions \
    && chmod +x install.sh \
    && sh install.sh \
    && rm -rf /tmp/extensions \
    # timezone
    && apk --no-cache add tzdata \
    && cp "/usr/share/zoneinfo/$TZ" /etc/localtime \
    && echo "$TZ" > /etc/timezone \
    #
    # Fix: https://github.com/docker-library/php/issues/240
    && apk add gnu-libiconv libstdc++ --no-cache --repository http://mirrors.aliyun.com/alpine/edge/community/ --allow-untrusted \
    #
    # Install composer and change it's cache home
    && curl -o /usr/bin/composer https://mirrors.aliyun.com/composer/composer.phar \
    && chmod +x /usr/bin/composer \
    && /usr/bin/composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \
    && mkdir -p /tmp/composer \
    #
    # php image's www-data user uid & gid are 82, change them to 1000 (primary user)
    && apk --no-cache add shadow && usermod -u 1000 www-data && groupmod -g 1000 www-data \
    # ---------- clear works ----------
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man 

ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php \
    COMPOSER_HOME=/tmp/composer

WORKDIR /mnt/www