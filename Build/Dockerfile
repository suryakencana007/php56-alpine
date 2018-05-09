FROM php:5.6-alpine


# Install gnu-libconv required by php5-iconv
RUN apk add --no-cache --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing gnu-libiconv
ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so

# Setup apache and php
RUN apk --update add --no-cache $PHPIZE_DEPS \
	git apache2 php5-apache2 curl memcached \
	php5-pcntl \
	php5-common \
    php5-json \
    php5-phar \
    php5-openssl \
    php5-mysql \
    php5-curl \
    php5-mcrypt \
    php5-memcache \
    php5-pdo_mysql \
    php5-ctype \
    php5-gd \
    php5-xml \
    php5-zlib \
    php5-dom \
    php5-iconv \
    && docker-php-ext-install pcntl pdo_mysql mysql json \
    && docker-php-ext-enable pcntl pdo_mysql mysql json

RUN pecl install mongodb \
    && docker-php-ext-enable mongodb
RUN pecl install apcu-4.0.11 \
    && docker-php-ext-enable apcu
RUN pecl install xdebug-2.5.5 \
    && docker-php-ext-enable xdebug
RUN apk add libssh2-dev \
    && pecl install ssh2 \
    && docker-php-ext-enable ssh2

RUN pecl install redis-2.2.8 \
    && docker-php-ext-enable redis

RUN curl -sS "https://getcomposer.org/installer" | php
RUN mv composer.phar /usr/local/bin/composer

RUN mkdir /run/apache2 \
    && mkdir -p /opt/utils

RUN rm -rf /var/cache/apk && mkdir -p /var/cache/apk

EXPOSE 80 443

ADD start.sh /opt/utils/

RUN chmod +x /opt/utils/start.sh

ENTRYPOINT ["/opt/utils/start.sh"]
