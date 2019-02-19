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

RUN docker-php-source extract && \
  apk add --update --no-cache autoconf g++ make && \
  pecl install redis && \
  docker-php-ext-enable redis && \
  docker-php-ext-install mysqli && \
  docker-php-ext-install pdo && \
  docker-php-ext-install pdo_mysql && \
  docker-php-ext-install mbstring && \
  docker-php-ext-install tokenizer && \
  docker-php-source delete

RUN rm -rf /var/cache/apk/* \
    && mkdir -p /var/www/html

COPY crontab /var/spool/cron/crontabs/laravel-schedule
COPY supervisord.conf /etc/supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]

WORKDIR /etc/supervisor/conf.d/
