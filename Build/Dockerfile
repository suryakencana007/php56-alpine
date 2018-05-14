FROM alpine:3.5

COPY rootfs /

RUN mkdir /app
# ensure www-data user exists
RUN set -x \
    && addgroup -g 82 -S www-data \
    && adduser -u 82 -D -S -G www-data www-data

RUN echo "@testing http://dl-4.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories \
    && apk add --update \
    wget \
    ca-certificates \
    openssh \
	apache2 \
    php5-apache2 \
	php5-gd \
    php5-exif \
    php5-json \
    php5-zlib \
    php5-bz2 \
    php5-bcmath \
    php5-xml \
    php5-intl \
    php5-phar \
    php5-openssl \
    php5-mcrypt \
    php5-dom \
    php5-pdo \
    php5-mysql \
    php5-ctype \
    php5-opcache \
    php5-memcache \
    php5-curl \
    su-exec \
    openssl-dev \
    bash \
    git

# Configure PHP
RUN chmod +x /sbin/build-php-extensions
RUN build-php-extensions

# Install composer

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/sbin --filename=composer \
    && php -r "unlink('composer-setup.php');" 

RUN mkdir /run/apache2 \
    && mkdir -p /opt/utils

# Cleanup
RUN apk del wget \
    && rm -rf /var/cache/apk/* \
    && rm -rf /tmp/* \
    && rm -rf /root/.composer/cache

EXPOSE 80 443

ADD start.sh /opt/utils/

RUN chmod +x /opt/utils/start.sh

ENTRYPOINT ["/opt/utils/start.sh"]
