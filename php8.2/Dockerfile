FROM php:8.2-fpm

# Update package repository and install required dependencies
RUN apt-get update && apt-get install -y \
    mariadb-client \
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
    libzip-dev \
    libmagickwand-dev

# Install and configure PHP extensions
RUN docker-php-ext-configure intl \
    && docker-php-ext-configure gd --with-jpeg --with-freetype

RUN pecl install imagick \
    && docker-php-ext-enable imagick

RUN docker-php-ext-install -j$(nproc) \
    bcmath \
    ctype \
    dom \
    exif \
    fileinfo \
    gd \
    intl \
    mysqli \
    pdo \
    pdo_mysql \
    xml \
    zip \
    pcntl

# Ensure all necessary extensions are enabled
RUN docker-php-ext-enable \
    bcmath \
    ctype \
    dom \
    exif \
    fileinfo \
    gd \
    imagick \
    intl \
    mysqli \
    pdo \
    xml \
    zip

# Copy composer from the composer image
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Configure user and working directory
ARG UNAME=nayeem
ARG UGROUP=nayeem
ARG UID=1000
ARG GID=1000
RUN useradd -ms /bin/bash $UNAME \
    && usermod --uid $UID $UNAME \
    && groupmod --gid $GID $UGROUP

RUN chown $UNAME:$UNAME -R /var/www \
    && chmod 777 -R /var/www

USER $UNAME
WORKDIR /var/www
