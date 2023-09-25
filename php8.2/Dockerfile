FROM php:8.2 AS build

RUN apt-get update

RUN mkdir -p /home/extensions

RUN pecl install mongodb

RUN PHP_EXT_DIR=$(php -r 'echo ini_get("extension_dir");') && cp $PHP_EXT_DIR/*mongodb.so /home/extensions/
RUN echo $(ls -al /home/extensions)

FROM php:8.2-fpm

RUN apt-get update
RUN apt-get install -y mariadb-client

RUN apt-get install -y \
		libfreetype6-dev \
		libjpeg62-turbo-dev \
		libpng-dev \
        autoconf \
        zlib1g-dev \
        libxml2-dev \
        git \
        zip \
        unzip \
        curl \
        openssl \
        libzip-dev

RUN docker-php-ext-configure intl
RUN docker-php-ext-configure gd \
                   --with-jpeg \
                   --with-freetype

RUN docker-php-ext-install -j$(nproc) bcmath \
        ctype \
        dom \
        fileinfo \
        mysqli \
        pdo \
        pdo_mysql \
        xml \
        gd \
        zip \
        exif

RUN docker-php-ext-enable bcmath \
        ctype \
        dom \
        fileinfo \
        pdo \
        xml \
        exif


RUN mkdir -p /home/extensions
COPY --from=build /home/extensions/mongodb.so /home/extensions
RUN PHP_EXT_DIR=$(php -r 'echo ini_get("extension_dir");') && cp /home/extensions/*mongodb.so $PHP_EXT_DIR
RUN rm -rf /home/extensions

RUN  docker-php-ext-install gd

RUN docker-php-ext-enable gd mongodb

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

ARG UNAME=nayeem
ARG UGROUP=nayeem
ARG UID=1000
ARG GID=1000
RUN useradd -ms /bin/bash $UNAME
RUN usermod  --uid $UID $UNAME
RUN groupmod --gid $GID $UGROUP

RUN chown $UNAME:$UNAME -R /var/www
RUN chmod 777 -R /var/www
USER $USER
WORKDIR /var/www