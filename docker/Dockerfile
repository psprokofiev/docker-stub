FROM php:8.0.11-fpm

ENV PHP_XDEBUG_VERSION 3.1.2
ENV COMPOSER_VERSION 2.2.5
ENV NODE_VERSION "16.x"

RUN apt-get update
RUN apt install -y apt-utils

# Install dependencies
RUN apt-get install -qq -y \
  curl \
  git \
  libzip-dev \
  zlib1g-dev \
  zip unzip

RUN apt install -y libmcrypt-dev libicu-dev libxml2-dev
RUN apt-get install -y libjpeg-dev libpng-dev libfreetype6-dev libjpeg62-turbo-dev
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/

# Install extensions
RUN docker-php-ext-install \
  gd \
  bcmath \
  pdo_mysql \
  pcntl \
  zip \
  pdo \
  ctype \
  tokenizer \
  fileinfo \
  xml \
  intl

# Redis
RUN pecl install -o -f redis \
  && rm -rf /tmp/pear \
  && docker-php-ext-enable redis

# XDebug
RUN pecl install xdebug-${PHP_XDEBUG_VERSION} \
  && docker-php-ext-enable xdebug \
  && echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/xdebug.ini \
  && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini

# Clear cache
RUN apt-get clean \
  && rm -rf /var/lib/apt/lists/*

# Node
RUN curl -fsSL https://deb.nodesource.com/setup_${NODE_VERSION} | bash -
RUN apt install -y nodejs
RUN mkdir /.npm && chmod -R 777 /.npm

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
  --install-dir=/usr/local/bin --version=${COMPOSER_VERSION} --filename=composer && chmod +x /usr/local/bin/composer

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# Fix tinker
RUN mkdir -p /.config/psysh && chmod -R 777 /.config

WORKDIR /var/www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
