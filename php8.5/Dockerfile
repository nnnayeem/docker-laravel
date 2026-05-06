FROM php:8.5-fpm

RUN apt-get update
RUN apt-get install -y mariadb-client && rm -rf /var/lib/apt

RUN --mount=type=cache,target=/var/cache/docker/php8.5-container-apt apt-get update && apt-get install -y \
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

RUN docker-php-ext-configure intl

RUN --mount=type=cache,target=/var/cache/docker/php8.5-container-gd docker-php-ext-configure gd \
                   --with-jpeg \
                   --with-freetype

RUN --mount=type=cache,target=/var/cache/docker/php8.5-container-ext-install docker-php-ext-install -j$(nproc) bcmath \
        ctype \
        fileinfo \
        mysqli \
        pdo \
        pdo_mysql \
        xml \
        gd \
        zip \
        exif \
        intl

RUN --mount=type=cache,target=/var/cache/docker/php8.5-container-ext-enable docker-php-ext-enable bcmath \
        ctype \
        fileinfo \
        mysqli \
        pdo \
        pdo_mysql \
        xml \
        gd \
        zip \
        exif \
        intl

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
