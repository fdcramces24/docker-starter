FROM php:8.2-fpm

# Install dependencies and PHP extensions
RUN apt-get update && apt-get install -y --no-install-recommends \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libzip-dev \
    unzip \
    procps \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd \
    && docker-php-ext-install pdo pdo_mysql zip

# Clear the cache to reduce image size
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www

# timezone environment
ENV TZ=UTC \
	LANG=en_US.UTF-8 \
	LANGUAGE=en_US:en \
	LC_ALL=en_US.UTF-8 \
	COMPOSER_ALLOW_SUPERUSER=1 \
	COMPOSER_HOME=/composer

COPY --from=composer:2.6 /usr/bin/composer /usr/bin/composer

# Update the trusted key store and install necessary packages
RUN apt-get update -o Acquire::Check-Valid-Until=false --fix-missing && \
	apt-get install -y --no-install-recommends ca-certificates && \
	apt-get update && \
	apt-get -y install --no-install-recommends \
	locales \
	git \
	unzip \
	libzip-dev \
	libicu-dev \
	libonig-dev \
	libjpeg-dev \
	libpng-dev \
	libfreetype6-dev \
	libwebp-dev \
	libxpm-dev \
	vim && \
	locale-gen en_US.UTF-8 && \
	localedef -f UTF-8 -i en_US en_US.UTF-8 && \
	docker-php-ext-configure gd \
	--with-jpeg \
	--with-freetype \
	--with-webp \
	--with-xpm && \
	docker-php-ext-install \
	intl \
	pdo_mysql \
	zip \
	bcmath \
	exif \
	pcntl \
	sockets \
	gd && \
	mkdir -p /tmp/pear/cache && \
	pecl channel-update pecl.php.net && \
	pecl clear-cache && \
	pecl install redis-5.3.4 && \
	docker-php-ext-enable redis && \
	docker-php-ext-install opcache && \
	composer config -g process-timeout 3600 && \
	composer config -g repos.packagist composer https://packagist.org

WORKDIR /var/www
