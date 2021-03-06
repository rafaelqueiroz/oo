FROM php:7.1-apache
MAINTAINER Potato Powered Software <support@potatopowered.net>

# update apt-get
RUN apt-get update

# install the required components
RUN apt-get install -y apt-utils libmcrypt-dev g++ libicu-dev libmcrypt4 libicu52 zlib1g-dev git libz-dev libmemcached-dev curl

# install the PHP extensions we need
RUN docker-php-ext-install intl
RUN docker-php-ext-install mcrypt
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install zip

#####ADD ADDITIONAL INSTALLS OR MODULES BELOW#########
RUN pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.var_display_max_depth=-1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.var_display_max_children=-1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.var_display_max_data=-1" >> /usr/local/etc/php/conf.d/xdebug.ini

RUN docker-php-ext-enable xdebug

#RUN apt-get update && apt-get install -y libmagickwand-dev --no-install-recommends
#RUN pecl install imagick && docker-php-ext-enable imagick

#####ADD ADDITIONAL INSTALLS OR MODULES ABOVE#########
RUN apt-get install vim -y

# cleanup after the installations
RUN apt-get purge --auto-remove -y libmcrypt-dev g++ libicu-dev zlib1g-dev
# delete the lists for apt-get as the take up space we do not need.
RUN rm -rf /var/lib/apt/lists/*

# install composer globally so that you can call composer directly
RUN curl -sSL https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

# enable apache rewrite
RUN a2enmod rewrite

# set www permissions
RUN usermod -u 1000 www-data

RUN apachectl restart