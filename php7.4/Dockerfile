FROM php:7.4-fpm

RUN apt-get update && apt-get install -y \
		libfreetype6-dev \
		libjpeg62-turbo-dev \
		libpng-dev \
        autoconf \
        zlib1g-dev \
        libxml2-dev \
        libzip-dev \
        git \
        zip \
        unzip \
        curl \
        openssl

RUN docker-php-ext-configure gd
RUN docker-php-ext-install -j$(nproc) bcmath \
    ctype \
    dom \
    fileinfo \
    mysqli \
    pdo \
    pdo_mysql \
    xml \
    gd \
    zip

RUN docker-php-ext-enable bcmath
RUN docker-php-ext-enable ctype
RUN docker-php-ext-enable dom
RUN docker-php-ext-enable fileinfo
RUN docker-php-ext-enable pdo
RUN docker-php-ext-enable xml
RUN docker-php-ext-enable gd
RUN docker-php-ext-enable pdo_mysql
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install zip

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


