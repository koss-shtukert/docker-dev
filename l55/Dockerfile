FROM ubuntu

RUN apt-get update -y && apt-get install -y \
        wget git vim telnet\
        tar unzip gzip bzip2 realpath \
        re2c g++ pkg-config curl autoconf make automake \
        libreadline6 libreadline6-dev readline-doc \
        libcurl4-openssl-dev libssl-dev libxslt1-dev \
        libfreetype6-dev libghc-text-icu-dev libicu-dev \
        libmemcached-dev libgearman-dev \
        imagemagick imagemagick-common libmagickwand-dev imagemagick-doc \
        mc ssmtp language-pack-en-base htop iputils-ping \
        software-properties-common \
        nginx \
        zip unzip

RUN apt-get remove -y apache2

RUN LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php
RUN apt-get update && apt-get install -y \
        php7.1 \
        php7.1-fpm \
        php7.1-mysql \
        php7.1-mbstring \
        php7.1-xml --force-yes \
        php7.1-curl \
        php7.1-json \
        php7.1-gd \
        php7.1-bcmath \
        php7.1-mongodb \
        php7.1-soap \
        php7.1-zip \
        php7.1-apc \
        php-xdebug \
        php-mysql

RUN ln -sf /usr/share/zoneinfo/CET /etc/localtime
RUN dpkg-reconfigure --frontend=noninteractive locales
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY config/php/pool/www.conf /etc/php/7.1/fpm/pool.d/www.conf
COPY config/php/php-fpm.conf /etc/php/7.1/fpm/php-fpm.conf
COPY config/php/php.ini /etc/php/7.1/fpm/php.ini

RUN rm -rf /var/www/*

ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV PHP_XDEBUG_ENABLED=1

ENTRYPOINT echo "\n172.17.0.1 l55.dev" >> /etc/hosts && service php7.1-fpm start && bash

WORKDIR /var/www/l55

EXPOSE 9999