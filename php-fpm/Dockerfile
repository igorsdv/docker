FROM debian:stretch-slim

ARG HOST_UID=1000
ARG PHP_VERSION=7.2

RUN apt-get update -q && apt-get install -yq --no-install-recommends wget gnupg ca-certificates apt-transport-https \
    && wget -q https://packages.sury.org/php/apt.gpg -O- | apt-key add - \
    && echo "deb https://packages.sury.org/php/ stretch main" > /etc/apt/sources.list.d/php.list

RUN apt-get update -q && apt-get install -yq --no-install-recommends \
    php${PHP_VERSION}-bcmath \
    php${PHP_VERSION}-bz2 \
    php${PHP_VERSION}-common \
    php${PHP_VERSION}-curl \
    php${PHP_VERSION}-fpm \
    php${PHP_VERSION}-gd \
    php${PHP_VERSION}-intl \
    php${PHP_VERSION}-mbstring \
    php${PHP_VERSION}-mysql \
    php${PHP_VERSION}-redis \
    php${PHP_VERSION}-simplexml \
    php${PHP_VERSION}-soap \
    php${PHP_VERSION}-uploadprogress \
    php${PHP_VERSION}-xdebug \
    php${PHP_VERSION}-xml \
    php${PHP_VERSION}-xsl \
    php${PHP_VERSION}-zip

RUN apt-get install -yq --no-install-recommends msmtp msmtp-mta

RUN apt-get -q autoclean && rm -r /var/lib/apt/lists/*

COPY conf/php-fpm.conf /etc/php/${PHP_VERSION}/fpm/php-fpm.conf
COPY conf/php.ini /etc/php/${PHP_VERSION}/fpm/php.ini
COPY conf/php.ini /etc/php/${PHP_VERSION}/cli/php.ini
COPY conf/xdebug.ini /etc/php/${PHP_VERSION}/mods-available/xdebug.ini
COPY bin/* /usr/local/bin

RUN phpdismod xdebug

RUN usermod -o -u ${HOST_UID} www-data && groupmod -o -g ${HOST_UID} www-data

RUN mkdir /var/www && chown www-data: /var/www

RUN ln -s /usr/sbin/php-fpm${PHP_VERSION} /usr/local/sbin/php-fpm

WORKDIR /srv/www

STOPSIGNAL SIGQUIT

EXPOSE 9000
CMD ["php-fpm"]
