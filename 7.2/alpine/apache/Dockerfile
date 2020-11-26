FROM alpine:3.9
MAINTAINER loouss.ls@gmail.com

# aliyun
#RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

RUN wget -O /etc/apk/keys/phpearth.rsa.pub https://repos.php.earth/alpine/phpearth.rsa.pub && echo "https://repos.php.earth/alpine/v3.9" >> /etc/apk/repositories

# Add basics first
RUN apk update && apk upgrade && apk add \
	bash apache2 php7.2-apache2 curl ca-certificates openssl php7.2 php7.2-phar php7.2-json php7.2-iconv php7.2-openssl tzdata openntpd

RUN apk update \
    && apk add --no-cache --virtual .build-deps php7.2-pear php7.2-dev zlib-dev re2c gcc g++ make \
    && curl -fsSL "https://pecl.php.net/get/xlswriter-1.3.6.tgz" -o xlswriter.tgz \
    && mkdir -p /tmp/xlswriter \
    && tar -xf xlswriter.tgz -C /tmp/xlswriter --strip-components=1 \
    && rm xlswriter.tgz \
    && cd /tmp/xlswriter \
    && phpize && ./configure --enable-reader && make && make install \
	&& rm -rf /tmp/xlswriter \
	&& apk del .build-deps \
	&& rm -rf /tmp/pear ~/.pearrc


# Setup apache and php
RUN apk add \
	php7.2-ftp \
	php7.2-xdebug \
	php7.2-mcrypt \
	php7.2-mbstring \
	php7.2-soap \
	php7.2-gmp \
	php7.2-pdo_odbc \
	php7.2-dom \
	php7.2-pdo \
	php7.2-zip \
	php7.2-mysqli \
	php7.2-sqlite3 \
	php7.2-pdo_pgsql \
	php7.2-bcmath \
	php7.2-gd \
	php7.2-odbc \
	php7.2-pdo_mysql \
	php7.2-pdo_sqlite \
	php7.2-gettext \
	php7.2-xml \
	php7.2-xmlreader \
	php7.2-xmlwriter \
	php7.2-tokenizer \
	php7.2-xmlrpc \
	php7.2-bz2 \
	php7.2-pdo_dblib \
	php7.2-curl \
	php7.2-ctype \
	php7.2-session \
	php7.2-redis \
	php7.2-exif \
	php7.2-intl \
	php7.2-fileinfo \
	php7.2-ldap \
	php7.2-apcu \
	php7.2-redis

# Problems installing in above stack
RUN apk add php7.2-simplexml

RUN cp /usr/bin/php7.2 /usr/bin/php \
    && rm -f /var/cache/apk/*

# Add apache to run and configure
RUN sed -i "s/#LoadModule\ rewrite_module/LoadModule\ rewrite_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_module/LoadModule\ session_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_cookie_module/LoadModule\ session_cookie_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_crypto_module/LoadModule\ session_crypto_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ deflate_module/LoadModule\ deflate_module/" /etc/apache2/httpd.conf \
    && sed -i "s#^DocumentRoot \".*#DocumentRoot \"/app/public\"#g" /etc/apache2/httpd.conf \
    && sed -i "s#/var/www/localhost/htdocs#/app/public#" /etc/apache2/httpd.conf \
	&& sed -i "s/Options Indexes FollowSymLinks/Options \-Indexes/" /etc/apache2/httpd.conf \
    && printf "\n<Directory \"/app/public\">\n\tAllowOverride All\n</Directory>\n" >> /etc/apache2/httpd.conf \
	&& sed -i "s/expose_php = On/expose_php \= Off/" /etc/php/7.2/php.ini \
	&& echo "extension=xlswriter.so" > /etc/php/7.2/conf.d/xlswriter.ini

RUN mkdir /app && mkdir /app/public && chown -R apache:apache /app && chmod -R 755 /app && mkdir bootstrap

EXPOSE 80
ENTRYPOINT ["httpd","-D","FOREGROUND"]