FROM alpine:3.10
LABEL maintainer="martin+docker@x8x.at"

ENV COMPOSER_HOME /tmp/composer
ENV COMPOSER_CACHE_DIR /tmp/composer/cache
ENV COMPOSER_ALLOW_SUPERUSER 1

RUN apk upgrade --update \
    && apk add --no-cache \
        bash \
        nginx \
        supervisor \
        composer \
        php7-cli \
        php7-fpm \
        php7-bz2 \
        php7-curl \
        php7-ctype \
        php7-dom \
        php7-exif \
        php7-fileinfo \
        php7-iconv \
        php7-imap \
        php7-intl \
        php7-mbstring \
        php7-mysqli \
        php7-opcache \
        php7-pdo \
        php7-pdo_mysql \
        php7-posix \
        php7-pspell \
        php7-redis \
        php7-simplexml \
        php7-xdebug \
        php7-xml \
        php7-zip \
    && adduser -u 82 -D -S -G www-data www-data \
    && mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.orig \
    && sed 's/user nginx/user www-data/' /etc/nginx/nginx.conf.orig > /etc/nginx/nginx.conf \
    && mkdir -p /run/nginx \
    && composer self-update \
    && composer global require hirak/prestissimo \
    && mkdir -p \
        /application \
        /application/src \
        /application/log \
        /application/log/nginx \
        /application/log/php \
        /application/log/supervisord \
        /application/run \
        /application/sockets \
        /application/sockets/php \
        /application/sockets/supervisord \
    && touch \
        /etc/php7/conf.d/zzz-override.ini \
        /application/log/error.log \
    && rm -f /etc/php7/conf.d/xdebug.ini \
    && chown -R www-data /application/log /application/run \
    && chown -R root:root /application/log/supervisord* \
    && chown www-data:www-data /application/sockets/php \
    && chown -R www-data /var/tmp/nginx


WORKDIR /application/src

# System stuff
COPY docker/supervisord.conf /etc/supervisord.conf
COPY docker/nginx/nginx.conf /etc/nginx/nginx.conf
COPY docker/nginx/default-site.conf /etc/nginx/conf.d/default.conf
COPY docker/php/php.ini /etc/php7/php.ini
COPY docker/php/php-fpm.conf /etc/php7/php-fpm.conf
COPY docker/php/pool.conf /etc/php7/php-fpm.d/www.conf
COPY docker/php/opcache.blacklist /etc/php7/conf.d/opcache.blacklist

# Entrypoint
COPY docker/entrypoint.sh /
COPY docker/entrypoint.d/ /entrypoint.d/
RUN chmod +x /entrypoint.d/*

EXPOSE 80

ENTRYPOINT ["/entrypoint.sh"]
CMD /usr/bin/supervisord -c /etc/supervisord.conf -n
