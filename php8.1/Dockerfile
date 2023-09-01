FROM php:8.1 AS build

RUN apt-get update
RUN apt-get install -y libgrpc-dev
RUN apt-get install -y libprotobuf-dev
RUN apt-get install -y autoconf
RUN apt-get install -y zlib1g-dev
#RUN apt-get install -y php-dev
#RUN apt-get install -y php-pear
RUN pecl install grpc protobuf

FROM php:8.1.13-fpm

RUN apt-get update

RUN apt-get install -y mariadb-client
#RUN apt-get install libfreetype6-dev
RUN apt-get install -y libjpeg62-turbo-dev
RUN apt-get install -y libpng-dev
RUN apt-get install -y autoconf
RUN apt-get install -y zlib1g-dev
RUN apt-get install -y libxml2-dev
RUN apt-get install -y git
RUN apt-get install -y zip
RUN apt-get install -y unzip
RUN apt-get install -y curl
RUN apt-get install -y openssl
RUN apt-get install -y libzip-dev

RUN docker-php-ext-configure intl
#RUN -docker-php-ext-configure gd \
#                   --with-jpeg \
#                   --with-freetype

RUN set -e; docker-php-ext-configure gd --with-jpeg
RUN docker-php-ext-install -j$(nproc) gd

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

RUN  docker-php-ext-install intl pcntl gd
COPY --from=build /usr/local/lib/php/extensions/no-debug-non-zts-20210902/grpc.so /usr/local/lib/php/extensions/no-debug-non-zts-20210902/
COPY --from=build /usr/local/lib/php/extensions/no-debug-non-zts-20210902/protobuf.so /usr/local/lib/php/extensions/no-debug-non-zts-20210902/
RUN docker-php-ext-enable grpc protobuf gd

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