FROM php:7.4-fpm-alpine
LABEL MAINTAINER="xiaojian688@qq.com"

# https://hub.docker.com/layers/library/php/7.4-fpm-alpine/images/sha256-4ca7fd8bea83cb12e43d192531576ca9a6b6a2d995763d3aaaee34f0d643f206?context=explore

# COPY composer.phar /usr/local/bin/composer
# RUN sed -i "s/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g" /etc/apk/repositories

ENV DEPS freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev

RUN \
    set -ex \
    && curl -sfL https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer \
    && chmod +x /usr/bin/composer \
    && composer self-update --clean-backups 2.2.21 \
    #begin
    && apk update && apk add --no-cache $PHPIZE_DEPS \
    && apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && docker-php-ext-install -j$(nproc) opcache \
    && docker-php-ext-install -j$(nproc) bcmath \
    && docker-php-ext-install -j$(nproc) mysqli \
    && docker-php-ext-install pcntl \
    && apk add --no-cache libmcrypt-dev && pecl install mcrypt && docker-php-ext-enable mcrypt \
    && apk add --no-cache libzip-dev && docker-php-ext-install -j$(nproc) zip \
    && apk add --no-cache  libpq-dev && docker-php-ext-install -j$(nproc) pgsql pdo_pgsql \
    && pecl install redis && docker-php-ext-enable redis \
    && docker-php-ext-install sockets && \
    #SWOOLE
    apk add --no-cache libstdc++ && \
    apk add --no-cache --virtual .build-deps curl-dev openssl-dev pcre-dev pcre2-dev zlib-dev && \
    docker-php-source extract && \
    mkdir /usr/src/php/ext/swoole && \
    curl -sfL https://github.com/swoole/swoole-src/archive/v4.8.13.tar.gz -o swoole.tar.gz && \
    tar xfz swoole.tar.gz --strip-components=1 -C /usr/src/php/ext/swoole && \
    docker-php-ext-configure swoole \
        --enable-http2   \
        --enable-mysqlnd \
        --enable-openssl \
        --enable-sockets --enable-swoole-curl --enable-swoole-json && \
    docker-php-ext-install -j$(nproc) swoole && \
    rm -f swoole.tar.gz $HOME/.composer/*-old.phar && \
    docker-php-source delete && \
    apk del .build-deps $PHPIZE_DEPS



ENV TZ Asia/Shanghai
RUN apk add tzdata && cp /usr/share/zoneinfo/${TZ} /etc/localtime \
    && echo ${TZ} > /etc/timezone \
    && apk del tzdata