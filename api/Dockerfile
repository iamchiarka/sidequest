FROM composer:2 AS vendor
WORKDIR /app
COPY composer.json composer.lock ./
RUN composer install --no-dev --no-scripts --no-autoloader --prefer-dist

FROM php:8.4-cli-alpine AS base

RUN apk add --no-cache postgresql-dev linux-headers brotli-dev $PHPIZE_DEPS \
    && docker-php-ext-install pdo_pgsql pcntl \
    && pecl install swoole redis \
    && docker-php-ext-enable swoole redis \
    && apk del $PHPIZE_DEPS

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html
COPY --from=vendor /app/vendor ./vendor
COPY . .
RUN composer dump-autoload --optimize --no-dev

EXPOSE 8000
CMD ["php", "artisan", "octane:start", "--server=swoole", "--host=0.0.0.0", "--port=8000"]
