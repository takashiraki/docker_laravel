FROM php:8.3-apache

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && apt-get update \
    && apt-get install -y unzip libpq-dev git vim sqlite3 libsqlite3-dev \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && docker-php-ext-install mysqli pdo_mysql opcache \
    && composer global require laravel/installer \
    && a2enmod rewrite

ENV PATH="/root/.composer/vendor/bin:${PATH}"

COPY ./php.ini /usr/local/etc/php/
COPY ./000-default.conf /etc/apache2/sites-available/

COPY --from=node:18.16 /usr/local/bin /usr/local/bin
COPY --from=node:18.16 /usr/local/lib /usr/local/lib

WORKDIR /var/www/html