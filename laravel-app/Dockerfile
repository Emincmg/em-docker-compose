## Composer dependencies.
FROM composer AS composer-build

WORKDIR /var/www/html/

COPY composer.json composer.lock /var/www/html/

RUN mkdir -p /var/www/html/database/{factories,seeds} \
    && composer install --no-dev --prefer-dist --no-scripts --no-autoloader --no-progress --ignore-platform-reqs

# NPM dependencies.
FROM node:18 as npm-builder

WORKDIR /var/www/html/

COPY package.json package-lock.json vite.config.js /var/www/html/
COPY resources /var/www/html/resources/
COPY public /var/www/html/public/

RUN npm install && npm run build

# Actual production image.
FROM php:8.1-fpm

WORKDIR /var/www/html/

ARG user
ARG uid

RUN apt-get update \
    && apt-get install --quiet --yes --no-install-recommends \
    libzip-dev \
    unzip \
    && docker-php-ext-install opcache zip pdo pdo_mysql

# The default production configuration of php:8.1-fpm image.
RUN mv $PHP_INI_DIR/php.ini-production $PHP_INI_DIR/php.ini

# Override with custom opcache settings
COPY ./php/opcache.ini $PHP_INI_DIR/conf.d/

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

COPY --chown=www-data --from=composer-build /var/www/html/vendor/ /var/www/html/vendor/
COPY --chown=www-data --from=npm-builder /var/www/html/public/ /var/www/html/public/
COPY --chown=www-data . /var/www/html/

RUN composer dump -o \
    && composer check-platform-reqs \
    && rm -f /usr/bin/composer