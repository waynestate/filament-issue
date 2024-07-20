FROM php:8.2.20-apache

RUN a2enmod rewrite
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

COPY 000-default.conf /etc/apache2/sites-available/000-default.conf
