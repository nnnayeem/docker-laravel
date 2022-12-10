FROM php:8.1.13-fpm

RUN apt-get update && apt-get install -y \
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
        openssl

RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j$(nproc) bcmath
RUN docker-php-ext-install -j$(nproc) ctype
RUN docker-php-ext-install -j$(nproc) dom
RUN docker-php-ext-install -j$(nproc) fileinfo
RUN docker-php-ext-install -j$(nproc) mysqli
RUN docker-php-ext-install -j$(nproc) pdo
RUN docker-php-ext-install -j$(nproc) pdo_mysql
RUN docker-php-ext-install -j$(nproc) xml

RUN docker-php-ext-enable bcmath
RUN docker-php-ext-enable ctype
RUN docker-php-ext-enable dom
RUN docker-php-ext-enable fileinfo
RUN docker-php-ext-enable pdo
RUN docker-php-ext-enable xml

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

RUN pecl install grpc
RUN pecl install protobuf

RUN docker-php-ext-enable grpc
RUN docker-php-ext-enable protobuf

