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
        zip unzip \
        mongodb \
        redis-server \
        default-jre \
        default-jdk

RUN apt-get remove -y apache2
RUN update-alternatives --config java
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y mysql-server

RUN ln -sf /usr/share/zoneinfo/CET /etc/localtime

RUN dpkg-reconfigure --frontend=noninteractive locales
RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.0.0.deb && dpkg -i elasticsearch-6.0.0.deb && touch /var/run/elasticsearch/elasticsearch.pid && chmod 777 -R /var/run/elasticsearch && chmod 777 -R /usr/share/elasticsearch/
RUN wget https://artifacts.elastic.co/downloads/kibana/kibana-6.0.0-amd64.deb && dpkg -i kibana-6.0.0-amd64.deb
RUN rm -rf elasticsearch-6.0.0.deb
RUN rm -rf kibana-6.0.0-amd64.deb
RUN rm -rf /var/www/* && rm -rf /etc/nginx/sites-enabled/*
RUN mkdir /etc/ssl/aih

COPY config/sysctl.conf /etc/sysctl.conf
COPY config/elasticsearch.yml /etc/elasticsearch/elasticsearch.yml
COPY config/kibana.yml /etc/kibana/kibana.yml
COPY config/mysqld.cnf /etc/mysql/mysql.conf.d/mysqld.cnf
COPY config/mongodb.conf /etc/mongodb.conf
COPY config/redis.conf /etc/redis/redis.conf

COPY config/kibana.conf /etc/nginx/sites-enabled/kibana
COPY config/l55.conf /etc/nginx/sites-enabled/l55

ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV PHP_XDEBUG_ENABLED=1

ENTRYPOINT echo "\n172.17.0.1 kibana.dev\n172.17.0.1 l55.dev" >> /etc/hosts && service nginx start && service mysql start && service redis-server start && service mongodb start && service elasticsearch start && service kibana start && bash

WORKDIR /var/www

EXPOSE 80 3306 27017 6379 9200 5601