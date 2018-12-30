
FROM php:7.2.10-cli-alpine3.8

ENV SWOOLE_VERSION=4.1.2 REDIS_VERSION=4.0.2

#Timezone and lib
RUN apk add -U tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && apk del tzdata

#RUN  apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS  \
#    && pecl install -o -f redis-4.0.1 \
#    && pecl install -o -f msgpack-2.0.3 \
#    && pecl install -o -f swoole-4.2.8 \
#    && docker-php-ext-enable redis swoole msgpack \
#    && apk del $PHPIZE_DEPS \
#    && rm -rf /tmp/pear ~/.pearrc \


RUN apk add --no-cache --virtual .build-deps \
    linux-headers \
    && pecl install -o -f swoole-4.2.8 \
    && pecl install -o -f redis-4.0.1 \
    && pecl install -o -f msgpack-2.0.3 \
    && docker-php-ext-enable redis swoole msgpack \
    && apk del .build-deps  linux-headers \
    && rm -rf /var/cache/apk/*

#RUN set -ex \
#        && cd /tmp \
#        && curl -SL "https://github.com/swoole/swoole-src/archive/v${SWOOLE_VERSION}.tar.gz" -o swoole.tar.gz \
##        && curl -SL "http://pecl.php.net/get/redis-${REDIS_VERSION}.tgz" -o redis.tgz \
##        && ls -alh \
##        && apk update \
#        && apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS \
#        # for swoole ext
#        && apk add --no-cache libaio \
#         linux-headers \
#         libaio-dev \
#         openssl-dev \
#         libstdc++ \
##         librdkafka-dev \
##        # php extension: redis
##        && pecl install redis.tgz \
##        && docker-php-ext-enable redis \
##        # php extension: pdo_mysql
##        && docker-php-ext-install pdo_mysql \
##        # php extension: bcmath
##        && docker-php-ext-install bcmath \
##        # php extension: swoole
##        && cd /tmp \
##        && mkdir -p swoole \
##        && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
##        && rm swoole.tar.gz \
##                && ( \
##                    cd swoole \
##                    && phpize \
##                    && ./configure --enable-mysqlnd --enable-coroutine --enable-openssl \
##                    && make -j$(nproc) && make install \
##                ) \
##                && rm -r swoole \
##                && docker-php-ext-enable swoole \
##        && apk del .phpize-deps \