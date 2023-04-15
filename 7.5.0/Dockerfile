FROM php:8.0-apache

LABEL maintainer="hcumbicusr@gmail.com"
LABEL version="7.5.0"
LABEL description="Vtiger crm"

ENV INSTALL_DIR /var/www/html
WORKDIR $INSTALL_DIR

# Use the default production configuration
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

RUN sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/apache2/apache2.conf

RUN requirements="zip wget curl libpng-dev zlib1g-dev libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev" \
    && apt-get update -y \
    && apt-get install -y $requirements

RUN apt-get update \
  && apt-get install -y zlib1g-dev libicu-dev g++ gcc libxml2 libxslt-dev libbz2-dev \
  && docker-php-ext-configure intl \
  && docker-php-ext-install intl \
  && docker-php-ext-install bz2

RUN docker-php-ext-install mysqli
# RUN docker-php-ext-install zip

# RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd

RUN a2enmod rewrite expires

# RUN docker-php-ext-configure mcrypt

# RUN docker-php-ext-install mcrypt
# RUN docker-php-ext-install mbstring
# RUN docker-php-ext-install zlib
RUN docker-php-ext-install fileinfo
# RUN docker-php-ext-install json
RUN docker-php-ext-install dom
# RUN docker-php-ext-install openssl
# RUN docker-php-ext-install ldap
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install soap

# install the PHP extensions we need
RUN apt-get update && apt-get install -y cron curl wget libpng-dev libjpeg-dev libkrb5-dev zlib1g-dev \
    # && docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
    && docker-php-ext-install gd mysqli \
    && apt-get -y install libssl-dev libc-client2007e-dev libkrb5-dev \
    && docker-php-ext-configure imap --with-imap-ssl --with-kerberos \
    && docker-php-ext-install imap opcache\
&& rm -rf /var/lib/apt/lists/*

# setting the recommended for vtiger
RUN { \
        echo 'display_errors=On'; \
        echo 'max_execution_time=0'; \
        echo 'error_reporting=E_WARNING & ~E_NOTICE & ~E_DEPRECATED & ~E_STRICT'; \
        echo 'log_errors=On'; \
        echo 'short_open_tag=Off'; \
        echo 'upload_max_filesize=128M'; \
        echo 'sql.safe_mode=Off'; \
        echo 'max_input_vars=10000'; \
        echo 'memory_limit=512M'; \
        echo 'post_max_size=256M'; \
        echo 'max_input_time=-1'; \
        echo 'upload_max_size=128M'; \
        echo 'register_globals=Off'; \
        echo 'output_buffering=On'; \
        echo 'allow_call_time_reference=On'; \
        echo 'suhosin.simulation=On'; \
    } > /usr/local/etc/php/conf.d/vtiger-recommended.ini

# setting the reccomended for apcache
# https://secure.php.net/manual/en/opcache.installation.php
RUN { \
        echo 'opcache.memory_consumption=128'; \
        echo 'opcache.interned_strings_buffer=8'; \
        echo 'opcache.max_accelerated_files=4000'; \
        echo 'opcache.revalidate_freq=60'; \
        echo 'opcache.fast_shutdown=1'; \
        echo 'opcache.enable_cli=1'; \
} > /usr/local/etc/php/conf.d/opcache-recommended.ini

RUN chsh -s /bin/bash www-data

# Add cron job
RUN crontab -l | { cat; echo "*/15 * * * * /var/www/html/cron/vtigercron.sh >> /var/log/cron.log 2>&1"; } | crontab -

EXPOSE 80

COPY ./vtigercrm .

RUN find -type f -exec chmod 644 {} \;
RUN find -type d -exec chmod 755 {} \;
RUN chmod -R 777 test
RUN chmod -R 775 storage
RUN chmod -R 775 logs
RUN chmod -R 775 user_privileges
RUN chown -R www-data:www-data *

# COPY ./vtigercrm/config.inc.php config.inc.php

VOLUME ["$INSTALL_DIR"]
