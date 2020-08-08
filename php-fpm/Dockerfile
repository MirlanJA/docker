FROM php:7.2-fpm

# Arguments defined in docker-compose.yml
ARG USER
ARG UID
ARG NODE_VERSION

USER root

# Install system dependencies
RUN apt-get update && apt-get install -y \
    apt-utils \
    git \
    zip \
    curl \
    unzip \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    libmagickwand-dev \
    libgmp-dev \
    imagemagick

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd mysqli json gmp \
    && pecl install -o -f redis && rm -rf /tmp/pear && docker-php-ext-enable redis \
    && pecl install imagick && docker-php-ext-enable imagick

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $UID -d /home/$USER $USER
RUN mkdir -p /home/$USER/.composer && \
    chown -R $USER:$USER /home/$USER


# Install NodeJS using NVM
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.5/install.sh | bash > /dev/null \
    && export NVM_DIR="$HOME/.nvm" > /dev/null \
    && [ -s "$NVM_DIR/nvm.sh" ] > /dev/null && . "$NVM_DIR/nvm.sh" > /dev/null \
    && nvm install $NODE_VERSION \
    && nvm use node

COPY ./laravel.ini /usr/local/etc/php/conf.d

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

# Set working directory
WORKDIR /var/www

USER $USER

EXPOSE 9000

CMD ["php-fpm"]