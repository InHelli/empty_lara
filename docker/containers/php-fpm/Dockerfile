FROM php:7.3-fpm

RUN apt-get update && apt-get install -y libzip-dev libmemcached-dev zlib1g-dev libpng-dev libxml2-dev libpq-dev locales
RUN apt-get install -y libmagickwand-dev imagemagick wget && \
    pecl install imagick && \
    docker-php-ext-enable imagick
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install zip
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd
RUN docker-php-ext-install dom
RUN docker-php-ext-install xml
RUN docker-php-ext-install simplexml
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install pdo
RUN docker-php-ext-install pdo pdo_pgsql

RUN echo "ru_RU ISO-8859-5 \nru_RU.UTF-8 UTF-8 " >> /etc/locale.gen
RUN locale-gen ru_RU
RUN locale-gen ru_RU.UTF-8

RUN pecl install xdebug-beta && \
    docker-php-ext-enable xdebug
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

ARG PUID
ENV PUID ${PUID}
ARG PGID
ENV PGID ${PGID}

RUN groupmod -g ${PGID} www-data && \
    usermod -u ${PUID} www-data

RUN touch /usr/local/etc/php/conf.d/uploads.ini \
    && echo "upload_max_filesize = 50M;" >> /usr/local/etc/php/conf.d/uploads.ini \
    && echo "memory_limit = 256M;" >> /usr/local/etc/php/conf.d/uploads.ini \
    && echo "post_max_size = 50M;" >> /usr/local/etc/php/conf.d/uploads.ini

RUN echo "deb http://ftp.us.debian.org/debian/ jessie main contrib non-free \
          deb-src http://ftp.us.debian.org/debian/ jessie main contrib non-free" >> /etc/apt/sources.list

RUN apt-get update && apt-get install ttf-mscorefonts-installer -y

