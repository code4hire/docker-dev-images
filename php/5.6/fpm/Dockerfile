FROM php:5.6-fpm-alpine

LABEL maintainer Srdjan Vranac <vranac@gmail.com>

# persistent / runtime deps
ENV PHPIZE_DEPS \
    autoconf \
    cmake \
    file \
    g++ \
    gcc \
    libc-dev \
    make \
    git \
    pkgconf \
    re2c

# https://pkgs.alpinelinux.org/packages?name=&branch=&repo=&arch=&maintainer=
RUN apk update \
    && apk add --no-cache --virtual .build-deps \
            $PHPIZE_DEPS \
    && apk add git \
            bash \
            curl \
            icu \
            icu-libs \
            icu-dev \
            postgresql-dev \
            icu-dev \
            freetype \
            libpng \
            libjpeg-turbo \
            freetype-dev \
            libpng-dev \
            libjpeg-turbo-dev \
            imagemagick \
            imagemagick-dev \
            libtool \
            pcre-dev \
            php5-mcrypt \
            php5-gd \
            php5-exif \
            php5-pdo_mysql \
            php5-pdo_pgsql \
            php5-curl \
            php5-intl \
            php5-bcmath \
            php5-dom \
            php5-zip \
            php5-xsl \
            php5-pcntl \
            php5-opcache \
            php5-calendar \
            php5-ctype \
            php5-gettext \
            php5-iconv \
            php5-json \
            php5-mysqli \
            php5-pgsql \
            php5-pdo_pgsql \
            php5-sockets \
    && no | pecl install imagick-3.4.4 xdebug-2.5.5 redis-4.3.0 && docker-php-ext-enable redis \
    && rm -rf /var/cache/apk/* \
    # Install Composer
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    # Install Symfony installer
    && curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony \
    && chmod a+x /usr/local/bin/symfony

ADD ./php-fpm.conf /etc/php-fpm/php-fpm.conf

ADD ./php-fpm/ /usr/local/etc/php/conf.d/

ADD ./php-xdebug/ /usr/local/etc/php/conf.d/

ADD ./php-configuration/ /usr/local/etc/php/conf.d/

RUN mkdir -p /usr/share/php-fpm
RUN mkdir -p /var/log/php-fpm
RUN mkdir -p /var/log/xdebug
RUN mkdir -p /var/log/xdebug/profiler_output

EXPOSE 9000

CMD ["php-fpm", "--allow-to-run-as-root", "--fpm-config", "/etc/php-fpm/php-fpm.conf"]
