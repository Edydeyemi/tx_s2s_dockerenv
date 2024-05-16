FROM php:8.2.0-apache

ADD --chmod=0755 https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN install-php-extensions gd xdebug bcmath csv intl pdo_mysql pdo_pgsql mysqli uuid imagick json zip curl xml openssl mbstring tokenizer

# RUN a2enmod ssl && a2enmod socache_shmcb
RUN a2enmod rewrite && a2enmod ssl && a2enmod socache_shmcb
RUN mkdir -p /etc/apache2/ssl

COPY ./certs/certificate.crt /etc/apache2/ssl/
COPY ./certs/private.key /etc/apache2/ssl/

EXPOSE 80
EXPOSE 443

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf && a2enmod proxy_fcgi setenvif && a2enmod rewrite && service apache2 restart

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer

COPY ./apache/000-default.conf /etc/apache2/sites-available/000-default.conf

RUN apt-get update && apt-get upgrade -y
RUN apt install zip unzip nano -y
RUN apt install iputils-ping -y
