FROM daocloud.io/library/php:7.2-zts-alpine3.8


#ENV NGINX_VERSION 1.15.2

ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php
RUN apk add --no-cache --repository http://dl-3.alpinelinux.org/alpine/edge/testing gnu-libiconv

#RUN addgroup -S www \
#  && adduser -D -S -h /var/cache/www -s /sbin/nologin -G www www \
#  && apk add --no-cache --virtual .build-deps \
#    autoconf \
#    gcc \
#    vim \
#    git \
#    libc-dev \
#    make \
#    openssl-dev \
#    pcre-dev \
#    zlib-dev \
#    linux-headers \
#    curl \
#    gnupg \
#    libxslt-dev \
#    gd-dev \
#    geoip-dev \
#    perl-dev
#
#COPY ./nginx-$NGINX_VERSION.tar.gz ./nginx.tar.gz
#
#RUN
#RUN mkdir -p /usr/src \
#    && tar -zxC /usr/src -f nginx.tar.gz \
#    && cd /usr/src/nginx-$NGINX_VERSION \
#    && ./configure --prefix=/usr/local/nginx \
#      --user=www --group=www \
#      --error-log-path=/var/log/nginx_error.log \
#      --http-log-path=/var/log/nginx_access.log \
#      --pid-path=/var/run/nginx.pid \
#      --with-pcre \
#      --with-http_ssl_module \
#      --without-mail_pop3_module \
#      --without-mail_imap_module \
#      --with-http_gzip_static_module && \
#      make && make install

RUN echo @testing http://nl.alpinelinux.org/alpine/edge/testing >> /etc/apk/repositories && \
    echo /etc/apk/respositories && \
    apk update && \
    apk add --no-cache bash \
    openssh-client \
    wget \
    curl \
    libcurl \
    augeas-dev \
    openssl-dev \
    ca-certificates \
    dialog \
    autoconf \
    make \
    gcc \
    musl-dev \
    linux-headers \
    libmcrypt-dev \
    libpng-dev \
    icu-dev \
    libpq \
    libxslt-dev \
    libffi-dev \
    freetype-dev \
    sqlite-dev \
    libjpeg-turbo-dev

RUN pecl install mcrypt-1.0.1 && \
    pecl install redis

RUN docker-php-ext-configure gd \
      --with-gd \
      --with-freetype-dir=/usr/include/ \
      --with-png-dir=/usr/include/ \
      --with-jpeg-dir=/usr/include/

RUN docker-php-ext-install pdo_mysql mysqli gd exif fileinfo intl json opcache

RUN docker-php-ext-enable redis.so && \
  docker-php-ext-enable mcrypt.so && \
  docker-php-source delete

COPY ./composer-setup.php ./composer-setup.php
RUN chmod +x composer-setup.php

RUN  php composer-setup.php && \
  php -r "unlink('composer-setup.php');" && \
  mv composer.phar /usr/local/bin/composer && \
  composer config -g repo.packagist composer https://packagist.phpcomposer.com && \
  composer self-update

#RUN /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf

ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#EXPOSE 8080
#EXPOSE 80
#EXPOSE 443

#RUN tail -f /etc/hosts


