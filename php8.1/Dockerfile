FROM php:8.1.13-fpm

RUN apt-get update && apt-get install -y mariadb-client && rm -rf /var/lib/apt

RUN --mount=type=cache,target=/var/cache/docker/php8.1-container-apt apt-get update && apt-get install -y \
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
RUN --mount=type=cache,target=/var/cache/docker/php8.1-container-gd docker-php-ext-configure gd \
                   --with-jpeg \
                   --with-freetype

RUN --mount=type=cache,target=/var/cache/docker/php8.1-container-ext-install docker-php-ext-install -j$(nproc) bcmath \
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

RUN --mount=type=cache,target=/var/cache/docker/php8.1-container-ext-enable docker-php-ext-enable bcmath \
        ctype \
        dom \
        fileinfo \
        pdo \
        xml \
        exif

RUN  --mount=type=cache,target=/var/cache/docker/php8.1-container-ext-intl-gd docker-php-ext-install intl pcntl gd
RUN --mount=type=cache,target=/var/cache/docker/php8.1-container-grpc MAKEFLAGS="-j $(nproc)" pecl install grpc protobuf \
        && docker-php-ext-enable grpc protobuf gd

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