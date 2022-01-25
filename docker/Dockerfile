FROM php:8.0.10-fpm

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
RUN docker-php-ext-install gd

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install \
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

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
  --install-dir=/usr/local/bin --filename=composer && chmod +x /usr/local/bin/composer

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# Add user for laravel application
#RUN groupadd -g 1000 1000
#RUN useradd -u 1000 -ms /bin/bash -g 1000 1000

# Copy existing application directory contents
#COPY --chown=1000:1000 . /var/www

# Change current user to www
#USER www

WORKDIR /var/www

#RUN chown -R 1000:1000 /var/www
#RUN chmod -R 777 /var/www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
