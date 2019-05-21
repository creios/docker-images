# CHOSING DEBIAN STRÈTCH WITH PHP AND APACHE
FROM php:7.2.6-apache-stretch

MAINTAINER Tim Tegeler <tegeler@creios.net>

# SETTING ENVIRONMENT
ENV ENVIRONMENT docker

# UPDATE APT-GET
RUN apt-get -qq update

# INSTALL REQUIREMENTS FOR INSTALLING YARN AND COMPOSER
RUN apt-get install -y \
        wget \
        curl \
        gnupg2

# INSTALL XSENDFILE APACHE MODULE
RUN apt-get install -y libapache2-mod-xsendfile

# INSTALL PHP EXTENSION DEPENDENCIES
# intl
RUN apt-get install -y libicu-dev
# gmp
RUN apt-get install -y libgmp-dev
# gd
RUN apt-get install -y libjpeg-dev libpng-dev libfreetype6-dev

# INSALL PHP EXTENSIONS
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install intl
RUN docker-php-ext-install gmp
RUN docker-php-ext-install zip
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd

# INSTALL XDEBUG
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

# ADD REPO TO INSTALL YARN
RUN apt-get install -y apt-transport-https
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

# INSTALL EVERYTHING USED FOR CI
RUN apt-get -qq update && apt-get install -y -o DPkg::Options::="--force-confold" \
        git \
        openssh-client \
        yarn

# INSTALL COMPOSER
COPY install-composer.sh .
RUN ./install-composer.sh && \
    rm install-composer.sh

# PHP CONFIG
COPY php-config/php-development.ini  /usr/local/etc/php/php.ini

# XDEBUG CONFIG
COPY php-config/20-xdebug.ini /usr/local/etc/php/conf.d/20-xdebug.ini

# APACHE CONFIG
COPY apache-config/000-default.conf /etc/apache2/sites-available/000-default.conf
COPY apache-config/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf
COPY apache-config/certs/dhparam.pem /etc/ssl/certs/dhparam.pem
COPY apache-config/certs/apache-selfsigned.crt /etc/ssl/certs/apache-selfsigned.crt
COPY apache-config/certs/apache-selfsigned.key /etc/ssl/private/apache-selfsigned.key

# APACHE FURTHER CONFIG
RUN echo 'ServerName localhost' >> /etc/apache2/apache2.conf
RUN a2enmod \
    rewrite \
    ssl
# ENABLE SSL CONFIG
RUN a2ensite default-ssl
# CREATE APACHE DOCUMENT ROOT DIR
RUN mkdir /var/www/public
# REMOVE STD DOCUMENT ROOT
RUN rm -r /var/www/html

WORKDIR /var/www

# HTTP
EXPOSE 80
# HTTPS
EXPOSE 443