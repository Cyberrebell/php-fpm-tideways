FROM php:7.4.2-fpm-alpine

WORKDIR /var/www

RUN mv ${PHP_INI_DIR}/php.ini-production ${PHP_INI_DIR}/php.ini

RUN set -xe \
    && apk add --no-cache --virtual .build-deps \
    $PHPIZE_DEPS \
    && cd /tmp && wget -O tideways.tar.gz https://github.com/tideways/php-xhprof-extension/archive/v5.0.2.tar.gz \
    && tar xfvz tideways.tar.gz && cd php-xhprof-extension-* && phpize && ./configure && make && make install \
    && pecl install mongodb \
    && docker-php-ext-enable opcache tideways_xhprof mongodb \
    && rm -rf /tmp/* /var/www/*

RUN echo 'tideways.auto_prepend_library=0' >> ${PHP_INI_DIR}/conf.d/docker-php-ext-tideways.ini
