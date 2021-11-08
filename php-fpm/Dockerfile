ARG IMAGE

FROM ${IMAGE}

USER root

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    zip \
    curl \
    unzip \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    libmagickwand-dev --no-install-recommends \
    libgmp-dev

# Install PHP extensions
RUN rm -rf /var/lib/apt/lists/* \
    && pecl install imagick && docker-php-ext-enable imagick \
    && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd mysqli json gmp \
    && pecl install -o -f redis && rm -rf /tmp/pear && docker-php-ext-enable redis \
    && docker-php-ext-configure zip --with-libzip && docker-php-ext-install zip \
    && docker-php-ext-configure intl && docker-php-ext-install intl \
    && pecl install xdebug && docker-php-ext-enable xdebug

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u 1000 -d /home/www www \
    && mkdir -p /home/www/.composer \
    && chown -R www:www /home/www

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && rm /var/log/lastlog /var/log/faillog

USER www

CMD ["php-fpm"]
