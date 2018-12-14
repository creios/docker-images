# CHOSING DEBIAN
FROM debian:8

MAINTAINER Tim Tegeler <tegeler@creios.net>

# SETTING ENVIRONMENT
ENV ENVIRONMENT docker

# FIX FOR GETTING DNS FAILURES
RUN echo "8.8.8.8" >  /etc/resolv.conf && echo "8.8.4.4" >>  /etc/resolv.conf

# INSTALL
RUN apt-get -qq update && apt-get install -y \
        apache2 \
        libapache2-mod-xsendfile \
        php5 \
	php5-intl \
	php5-gmp \
        php5-mysql \
        php5-gd \
        make \
        php5-dev \
        php-pear \
        pkg-config \
        libmagickwand-dev && pecl install imagick

# APACHE
ADD ./000-default.conf /etc/apache2/sites-available/000-default.conf
RUN echo 'ServerName localhost' >> /etc/apache2/apache2.conf
RUN a2ensite 000-default
RUN a2enmod \
    rewrite \
    xsendfile
RUN ln -sf /dev/stdout /var/log/apache2/access.log && ln -sf /dev/stderr /var/log/apache2/error.log

# PHP
RUN echo 'extension=imagick.so' >> /etc/php5/apache2/conf.d/imagick.ini && \
    echo 'extension=imagick.so' >> /etc/php5/cli/conf.d/imagick.ini
RUN sed -i "s/error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT/error_reporting = E_ALL/" /etc/php5/apache2/php.ini && \
    sed -i "s/display_errors = Off/display_errors = On/" /etc/php5/apache2/php.ini && \
    sed -i "s/display_startup_errors = Off/display_startup_errors = On/" /etc/php5/apache2/php.ini && \
    sed -i "s/post_max_size = 8M/post_max_size = 100M/" /etc/php5/apache2/php.ini && \
    sed -i "s/upload_max_filesize = 2M/upload_max_filesize = 100M/" /etc/php5/apache2/php.ini && \
    sed -i "s/;always_populate_raw_post_data = -1/always_populate_raw_post_data = -1/" /etc/php5/apache2/php.ini

EXPOSE 80
COPY httpd-foreground /usr/local/bin/
CMD ["httpd-foreground"]
