FROM php:7.3-fpm

RUN apt-get update && apt-get install -y supervisor libzip-dev libmemcached-dev zlib1g-dev libpng-dev libxml2-dev libpq-dev
RUN apt-get install -y libmagickwand-dev imagemagick && pecl install imagick && \
    docker-php-ext-enable imagick
RUN apt-get install -y --fix-missing postgresql-client
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
RUN docker-php-ext-install pcntl

# Add a non-root user to prevent files being created with root permissions on host machine.
ARG PUID
ENV PUID ${PUID}
ARG PGID
ENV PGID ${PGID}


# always run apt update when start and after add new source list, then clean up at end.
RUN groupadd -g ${PGID} commander && \
    useradd -u ${PUID} -g commander -m commander && \
    usermod -p "*" commander
RUN touch /usr/local/etc/php/conf.d/uploads.ini \
    && echo "upload_max_filesize = 50M;" >> /usr/local/etc/php/conf.d/uploads.ini \
        && echo "memory_limit = 256M;" >> /usr/local/etc/php/conf.d/uploads.ini

COPY supervisord.conf /etc/supervisord.conf
COPY supervisord.d /etc/supervisord.d
WORKDIR /var/www/application
CMD ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]

