FROM php:7-alpine

MAINTAINER REN YANJI <laggards@hotmail.com>

ENV TIMEZONE  Asia/Shanghai

RUN apk --update add wget \
  curl \
  git \
  openssl-dev \
  build-base \
  libmemcached-dev \
  libmcrypt-dev \
  libxml2-dev \
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

RUN docker-php-ext-configure pdo_pgsql

RUN docker-php-ext-install mysqli mbstring pdo pdo_mysql pdo_pgsql mcrypt tokenizer xml
RUN pecl channel-update pecl.php.net && pecl install memcached && docker-php-ext-enable memcached

RUN rm /var/cache/apk/* \
    && mkdir -p /var/www/html

COPY crontab /var/spool/cron/crontabs/laravel-schedule
COPY supervisord.conf /etc/supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]

WORKDIR /etc/supervisor/conf.d/
