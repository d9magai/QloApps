FROM php:8.4-apache

ENV mysql_version=8.0 \
    php_version=7.2 \
    file_uploads=On \
    allow_url_fopen=On \
    memory_limit=512M \
    max_execution_time=240 \
    upload_max_filesize=200M \
    post_max_size=400M \
    max_input_vars=1500

RUN apt update \
    && apt install -y libpng-dev libjpeg-dev libfreetype6-dev libzip-dev libicu-dev libxml2-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd mysqli pdo pdo_mysql zip intl soap \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && a2enmod rewrite headers \
    && rm -rf /var/lib/apt/lists/*

ARG HOST_UID
ARG HOST_GID
RUN groupmod -g ${HOST_GID} www-data && \
    usermod  -u ${HOST_UID} -g ${HOST_GID} www-data && \
    chown -R www-data:www-data /var/www/html

WORKDIR /var/www/html
RUN chown -R www-data:www-data /var/www/html \
    && find /var/www/html -type f -exec chmod 644 {} \; \
    && find /var/www/html -type d -exec chmod 755 {} \;

COPY docker/php/php.ini /usr/local/etc/php/php.ini
COPY docker/apache/000-default.conf /etc/apache2/sites-enabled/000-default.conf
COPY docker/apache/apache2.conf /etc/apache2/apache2.conf

EXPOSE 80

