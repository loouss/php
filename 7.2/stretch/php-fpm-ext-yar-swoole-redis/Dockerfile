
FROM php:7.2-fpm

MAINTAINER loouss <loouss.ls@gmail.com>

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
# pecl install yar error 'reinstall libcurl'
        libcurl4-gnutls-dev && \
        apt autoremove && apt clean

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install \
    iconv \
    gd \
    pdo_mysql \
    mysqli \
    iconv \
    mbstring \
    json \
    opcache \
    sockets \
    pcntl \
    curl
#    && \
#    echo "opcache.enable_cli=1" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini

RUN pecl install redis-4.0.1 \
    && pecl install yar-2.0.4 \
    && pecl install swoole-4.2.8 \
    && docker-php-ext-enable redis yar swoole


# set China timezone
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo 'Asia/Shanghai' > /etc/timezone && \
    echo "[Date]\ndate.timezone=Asia/Shanghai" > /usr/local/etc/php/conf.d/timezone.ini
