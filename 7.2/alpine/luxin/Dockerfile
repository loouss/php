FROM loouss/php:7.2.17-apache-alpine

RUN apk add --no-cache --virtual .build-deps \
		autoconf \
        dpkg-dev dpkg \
        g++ \
        gcc \
        libc-dev \
        make \
        pkgconf \
        re2c \
        zip \
        libzip-dev \
    && pecl install -o -f zip-1.17.1 \
    && apk del --no-network .build-deps \
#    && docker-php-ext-enable zip
