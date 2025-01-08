# Get the latest PHP official image from Docker Hub
FROM php:8.4-apache

# Update package list and install required packages
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libzip-dev \
    zip \
    unzip \
    git

# Enable Apache mod_rewrite module
RUN a2enmod rewrite

# Set the Apache document root
ENV APACHE_DOCUMENT_ROOT /var/www/html/public

# Install PHP extensions with pecl
RUN pecl install xdebug \
	&& docker-php-ext-enable xdebug

# Install PHP extensions with docker
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd pdo pdo_mysql

# Install Composer globally
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy apache2 configuration file
COPY .docker/php/apache-config.conf  /etc/apache2/sites-available/000-default.conf

# Copy the xdebug configuration file
COPY .docker/php/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/

# Set the working directory
WORKDIR /var/www/html

# Copy the application files to the working directory
COPY --chown=www-data:www-data ../../ /var/www/html

# Expose port 80 for Apache.
EXPOSE 80

# Start Apache web server.
CMD ["apache2-foreground"]

