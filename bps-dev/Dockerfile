FROM creios/creiwork:v4-dev

MAINTAINER Tim Tegeler <tegeler@creios.net>

ENV ENVIRONMENT docker-dev

COPY ./sources.list /etc/apt/

RUN apt-get -qq update && \
    apt-get install -y \
        libmagickwand-dev \
        links \
        libcurl4-openssl-dev \
        libxml2-dev \
        wget \
        gcc \
        make \
        autoconf \
        libc-dev \
        pkg-config\
        libgpgme11-dev

## Composer
COPY install-composer.sh .
RUN ./install-composer.sh && \
    rm install-composer.sh

RUN docker-php-ext-install calendar
RUN docker-php-ext-install gd
RUN docker-php-ext-install curl
RUN docker-php-ext-install soap
RUN docker-php-ext-install zip
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install bcmath

RUN pecl install imagick
RUN pecl install gender
RUN pecl install gnupg

RUN wget --no-verbose -O /tmp/pdflib.tar.gz "https://www.pdflib.com/binaries/PDFlib/912/PDFlib-9.1.2p1-Linux-x86_64-php.tar.gz" && \
    tar -zxf /tmp/pdflib.tar.gz -C /tmp "PDFlib-9.1.2p1-Linux-x86_64-php/bind/php/php-720-nts/php_pdflib.so" && \
    mv "/tmp/PDFlib-9.1.2p1-Linux-x86_64-php/bind/php/php-720-nts/php_pdflib.so" "$(php -r 'echo ini_get("extension_dir");')/" && \
    rm -rf "/tmp/PDFlib-9.1.2p1-Linux-x86_64-php"
