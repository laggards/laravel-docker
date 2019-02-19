FROM php:7-alpine

MAINTAINER REN YANJI <laggards@hotmail.com>

ENV TIMEZONE  Asia/Shanghai

RUN apk --update add wget \
  git \
  build-base \
  libmemcached-dev \
  libmcrypt-dev \
  postgresql-dev \
  libpng-dev \
  zlib-dev \
  autoconf \
  cyrus-sasl-dev \
  libgsasl-dev \
  supervisor \
  tzdata
RUN cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
    echo "${TIMEZONE}" > /etc/timezone

RUN pecl channel-update pecl.php.net && pecl install memcached && docker-php-ext-enable memcached
    
RUN rm /var/cache/apk/* \
    && mkdir -p /var/www/html

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

COPY crontab /var/spool/cron/crontabs/laravel-schedule
COPY supervisord.conf /etc/supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]

WORKDIR /etc/supervisor/conf.d/
