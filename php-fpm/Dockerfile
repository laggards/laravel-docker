FROM php:7-fpm-alpine

MAINTAINER REN YANJI <laggards@hotmail.com>

ENV MEMCACHED_DEPS zlib-dev libmemcached-dev cyrus-sasl-dev
# Install dependencies
RUN apk --no-cache --update add libxml2-dev curl-dev libmemcached-dev zlib libpng-dev postgresql-dev libjpeg-turbo-dev freetype-dev && \
    rm -rf /tmp/* && \
    rm -rf /var/cache/apk/*

RUN set -xe \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
    && apk add --no-cache --update --virtual .memcached-deps $MEMCACHED_DEPS \
    && pecl install memcached \
    && echo "extension=memcached.so" > /usr/local/etc/php/conf.d/20_memcached.ini \
    && rm -rf /usr/share/php7 \
    && rm -rf /tmp/* \
    && apk del .memcached-deps .phpize-deps

# Configure PHP extensions
RUN docker-php-ext-configure json && \
    docker-php-ext-configure exif && \
    docker-php-ext-configure session && \
    docker-php-ext-configure ctype && \
    docker-php-ext-configure tokenizer && \
    docker-php-ext-configure simplexml && \
    docker-php-ext-configure dom && \
    docker-php-ext-configure mbstring && \
    docker-php-ext-configure zip && \
    docker-php-ext-configure pdo && \
    docker-php-ext-configure pdo_mysql && \
    docker-php-ext-configure pdo_pgsql && \
    docker-php-ext-configure mysqli && \
    docker-php-ext-configure curl && \
    docker-php-ext-configure iconv && \
    docker-php-ext-configure xml && \
    docker-php-ext-configure phar && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

# Build and install PHP extensions
RUN docker-php-ext-install json \
    sockets \
    soap  \
    exif \
    opcache \
    session \
    ctype \
    tokenizer \
    simplexml \
    dom \
    mbstring \
    zip \
    pdo \
    pdo_mysql \
    pdo_pgsql \
    mysqli \
    curl \
    iconv \
    xml  \
    phar \
    gd

RUN docker-php-source delete
