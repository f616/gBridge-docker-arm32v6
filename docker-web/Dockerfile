#
# Use build context ../ !!!!
#

FROM php:7-apache

WORKDIR /var/www/html/

COPY ./gbridge-src/web/ /var/www/html/
COPY ./docker-web/laravel-dotenv /var/www/html/.env
COPY ./docker-web/apache-config.conf /etc/apache2/sites-available/apache-gbridge.conf

RUN chown -R www-data:www-data /var/www/

#set the new document root -> not necessary since we are overwriting the config
#ENV APACHE_DOCUMENT_ROOT /var/www/html/public/
#RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
#RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

#apache config
RUN a2enmod rewrite
RUN a2dissite 000-default
RUN a2ensite apache-gbridge

ENV DEBIAN_FRONTEND noninteractive 
RUN apt-get update

RUN apt-get install -y zlib1g zlib1g-dev
RUN docker-php-ext-install zip

#install composer for laravel
ENV COMPOSER_ALLOW_SUPERUSER 1
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && ln -s $(composer config --global home) /root/composer
ENV PATH $PATH:/root/composer/vendor/bin7

RUN su -s /bin/bash -c "composer install" www-data
RUN su -s /bin/bash -c "composer update" www-data