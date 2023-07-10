# Imagen para entorno PHP
FROM php:8.1.19-fpm

# Local
ENV TZ=Asia/Tokyo

COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer
COPY --from=node:16-slim /usr/local/bin /usr/local/bin
COPY --from=node:16-slim /usr/local/lib/node_modules /usr/local/lib/node_modules

RUN apt-get update && apt-get install -y \
    git \
    iputils-ping \
    net-tools \
    zlib1g-dev \
    unzip \
    libzip-dev \
    libfreetype6-dev libjpeg62-turbo-dev libpng-dev libmcrypt-dev \
    imagemagick \
    libmagickwand-dev \
    libicu-dev \
    && docker-php-ext-install gd zip intl exif
 
# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
 
# PHP's DB setting
RUN docker-php-ext-install pdo_mysql && docker-php-ext-enable pdo_mysql 

# Install ext-imagick
RUN pecl install imagick && docker-php-ext-enable imagick

# Install opcache
RUN docker-php-ext-install opcache && docker-php-ext-enable opcache

# Install GD
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd

# Building application
ENV NODE_ENV=development

WORKDIR /var/www