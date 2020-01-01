FROM php:7.0-alpine

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
            nmap \
            icu-libs \
            icu-dev \
            gettext-dev \
            postgresql-dev \
            icu-dev \
            freetype \
            libpng \
            libjpeg-turbo \
            freetype-dev \
            libpng-dev \
            libjpeg-turbo-dev \
            libxslt-dev \
            libxml2-dev \
            libsodium-dev \
            imagemagick \
            imagemagick-dev \
            libtool \
            pcre-dev

RUN docker-php-ext-configure zip \
    && docker-php-ext-install zip \
    && docker-php-ext-configure calendar \
    && docker-php-ext-install calendar \
    && docker-php-ext-configure gd \
    && docker-php-ext-install gd \
    && docker-php-ext-configure exif \
    && docker-php-ext-install exif \
    && docker-php-ext-configure pdo_mysql \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-configure pdo_pgsql \
    && docker-php-ext-install pdo_pgsql \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-configure bcmath \
    && docker-php-ext-install bcmath \
    && docker-php-ext-configure xsl \
    && docker-php-ext-install xsl \
    && docker-php-ext-configure pcntl \
    && docker-php-ext-install pcntl \
    && docker-php-ext-configure opcache \
    && docker-php-ext-install opcache \
    && docker-php-ext-configure gettext \
    && docker-php-ext-install gettext \
    && docker-php-ext-configure mysqli \
    && docker-php-ext-install mysqli \
    && docker-php-ext-configure pgsql \
    && docker-php-ext-install pgsql \
    && docker-php-ext-configure sockets \
    && docker-php-ext-install sockets \
    && pecl install imagick-3.4.4 xdebug-2.9.0 igbinary redis \
    && docker-php-ext-enable xdebug \
    && docker-php-ext-enable imagick \
    && docker-php-ext-enable igbinary redis \
    && rm -rf /var/cache/apk/* \
    # Install Composer
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    # Install Symfony installer
    && curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony \
    &&  chmod a+x /usr/local/bin/symfony \
    # create aliases
    && echo "alias php-debug='/usr/local/bin/php -dzend_extension=/usr/lib/php7/modules/xdebug.so'" >> ~/.bashrc \
    && echo "alias php-profiler='XDEBUG_CONFIG=\"profiler_enable=1\" /usr/local/bin/php -dzend_extension=/usr/lib/php7/modules/xdebug.so'" >> ~/.bashrc

# configure extensions
ADD ./php-configuration/ /usr/local/etc/php/conf.d/
ADD ./php-xdebug/xdebug.ini ./php-xdebug/xdebug-profiler.ini /usr/local/etc/php/conf.d/

RUN mkdir -p /var/log/xdebug
RUN mkdir -p /var/log/xdebug/profiler_output
