FROM daocloud.io/library/php:7.2-fpm-alpine3.8

ENV PHP_LIB openssh-client \
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


#PHP
ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php
RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone && \
    apk add --no-cache --repository http://dl-3.alpinelinux.org/alpine/edge/testing gnu-libiconv && \
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \
    echo /etc/apk/respositories && \
    apk update && \
    apk add --no-cache bash \
    ${PHP_LIB}


RUN docker-php-ext-configure gd \
      --with-gd \
      --with-freetype-dir=/usr/include/ \
      --with-png-dir=/usr/include/ \
      --with-jpeg-dir=/usr/include/ && \

    docker-php-ext-install pdo_mysql mysqli gd exif fileinfo intl json opcache && \
    pecl install mcrypt-1.0.1 && \
    pecl install redis && \
#    pecl install swoole && \
    docker-php-ext-enable mcrypt.so && \
    docker-php-ext-enable redis.so && \
#    docker-php-ext-enable swoole.so && \
    docker-php-source delete

#COPY ./composer-setup.php ./composer-setup.php
#RUN chmod +x composer-setup.php
#
#RUN  php composer-setup.php && \
#  php -r "unlink('composer-setup.php');" && \
#  mv composer.phar /usr/local/bin/composer && \
#  composer config -g repo.packagist composer https://packagist.phpcomposer.com && \
#  composer self-update

#RUN /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf


#EXPOSE 9000

#RUN tail -f /etc/hosts


