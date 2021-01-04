FROM php:7.3-fpm

RUN apt-get update && apt-get install -y --no-install-recommends \
  autoconf \
  build-essential \
  curl \
  zlib1g-dev \
  libzip-dev \
  unzip \
  zip \
  libmagick++-dev \
  libmagickwand-dev \
  libpq-dev \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  libpng-dev

# Locales
RUN apt-get update && apt-get install -y \
    locales \
    && echo '' >> /usr/share/locale/locale.alias \
    && sed -i 's/# pt_BR.UTF-8 UTF-8/pt_BR.UTF-8 UTF-8/' /etc/locale.gen \
    && locale-gen

ENV \
    MAINTAINER="Dev OneNext" \
    EMAIL=dev@onenext.com.br \
    TERM=xterm \
    TZ=America/Campo_Grande \
    PS1='\u@\h:\w\$ ' \
    DOMAIN=intranet.br

RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/America/Campo_Grande /etc/localtime
RUN "date"

RUN docker-php-ext-configure gd \
  --with-png-dir=/usr/include/ \
  --with-jpeg-dir=/usr/include/ \
  --with-freetype-dir=/usr/include/

RUN docker-php-ext-configure zip --with-libzip

RUN docker-php-ext-install gd intl pdo_mysql pdo_pgsql mysqli zip sodium mbstring \
  && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN docker-php-ext-install sockets

RUN apt-get update && apt-get install -y \
    libmagickwand-dev
RUN pecl install imagick
RUN docker-php-ext-enable imagick

RUN pecl install xdebug && docker-php-ext-enable xdebug

RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/bin/composer

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get install -y nodejs
RUN npm install -g sass