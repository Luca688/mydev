FROM php:7.4.16-fpm

RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/' /etc/apt/sources.list && \
    sed -i 's/security.debian.org/mirrors.aliyun.com/' /etc/apt/sources.list && \
    sed -i 's/security-cdn.debian.org/mirrors.aliyun.com/' /etc/apt/sources.list

RUN set -eux; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
    curl \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libwebp-dev \
    libmcrypt-dev \
    libonig-dev; \
    rm -rf /var/lib/apt/lists/*

RUN set -eux; \
    docker-php-ext-install bcmath;\
    docker-php-ext-install calendar;\
    docker-php-ext-install exif;\
    docker-php-ext-install gettext;\
    docker-php-ext-install sockets;\
    docker-php-ext-install dba;\
    docker-php-ext-install mysqli;\
    docker-php-ext-install pcntl;\
    docker-php-ext-install pdo_mysql;\
    docker-php-ext-install shmop;\
    docker-php-ext-install sysvmsg;\
    docker-php-ext-install sysvsem;\
    docker-php-ext-install sysvshm;\
    docker-php-ext-install pdo_mysql; \
    docker-php-ext-install pdo_pgsql; \
    docker-php-ext-configure gd \
    --prefix=/usr \
    --with-jpeg \
    --with-webp \
    --with-freetype; \
    docker-php-ext-install gd;

# soap wddx xmlrpc tidy xsl
RUN apt-get update && \
    apt-get install -y --no-install-recommends libxml2-dev libtidy-dev libxslt1-dev && \
    rm -r /var/lib/apt/lists/* && \
    docker-php-ext-install -j$(nproc) soap xmlrpc tidy xsl
    # zip
RUN     apt-get update && \
    apt-get install -y --no-install-recommends libzip-dev && \
    rm -r /var/lib/apt/lists/* && \
    docker-php-ext-install -j$(nproc) zip

# memcached
RUN apt-get update && \
    apt-get install -y --no-install-recommends zlib1g-dev libmemcached-dev && \
    rm -r /var/lib/apt/lists/* && \
    pecl install memcached && \
    docker-php-ext-enable memcached

# redis
RUN pecl install redis-5.0.0 && docker-php-ext-enable redis

# opcache
RUN docker-php-ext-configure opcache --enable-opcache && docker-php-ext-install opcache


RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog && \
    docker-php-source delete

RUN groupmod -o -g 1000 www-data && \
    usermod -o -u 1000 -g www-data www-data

CMD ["php-fpm"]

EXPOSE 9000
