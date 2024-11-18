FROM php:8.3.13-fpm-alpine3.20

# Linux Headers are for xdebug
RUN apk add --no-cache $PHPIZE_DEPS \
    openrc \
    linux-headers \
    git \
    libzip-dev \
    nginx

RUN pecl install redis-6.1.0 \
    xdebug-3.3.2 \
    && rm -rf /tmp/pear

RUN docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install zip \
    && docker-php-ext-enable redis \
    && docker-php-ext-enable xdebug

COPY --from=composer:2.8.2 /usr/bin/composer /usr/bin/composer

COPY nginx.default.conf /etc/nginx/http.d/default.conf

WORKDIR /var/www/public
EXPOSE 80

CMD nginx -g "daemon off;" | php-fpm -D
