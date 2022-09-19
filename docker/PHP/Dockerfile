FROM php:7.4-fpm

USER root
# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    cron \
    libpq-dev \
    libxml2-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    libzip-dev \
    libonig-dev \
    graphviz \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    sudo \
    git \
    curl \
    whois \
    nodejs \
    npm \
    libyaml-dev

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install pdo
RUN docker-php-ext-install pdo_pgsql
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install zip
RUN docker-php-ext-install exif
RUN docker-php-ext-install pcntl
RUN docker-php-ext-install gd
RUN docker-php-ext-install soap
RUN pecl install redis && docker-php-ext-enable redis
#RUN docker-php-ext-install opcache

RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.mode=coverage" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -p "$(mkpasswd --method=sha-512 'www')" -ms /bin/bash -g www www
RUN usermod -a -G sudo www

# Copy existing application directory permissions
COPY --chown=www:www . /var/www

## Set working directory
WORKDIR /var/www/

# Change current user to www
USER www

#RUN crontab

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
