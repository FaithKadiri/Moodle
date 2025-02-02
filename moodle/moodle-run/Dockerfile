# Use the official PHP 8.3 image as a base
FROM php:8.3-apache

# Install necessary packages and extensions
RUN apt-get update && \
    apt-get install -y --no-install-recommends unzip git curl libzip-dev libjpeg-dev libpng-dev libfreetype6-dev libicu-dev libxml2-dev

# Configure PHP extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install mysqli zip gd intl soap exif && \
    docker-php-ext-enable mysqli zip gd intl soap exif

# Clean up to reduce image size
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Clone the Moodle repository and check out the desired branch
RUN git clone git://git.moodle.org/moodle.git && \
    cd moodle && \
    git branch --track MOODLE_405_STABLE origin/MOODLE_405_STABLE && \
    git checkout MOODLE_405_STABLE && \
    cp -rf ./* /var/www/html/

# Set PHP configurations
RUN echo "max_input_vars=5000" >> /usr/local/etc/php/conf.d/docker-php-moodle.ini && \
    echo "opcache.enable=1" >> /usr/local/etc/php/conf.d/docker-php-moodle.ini

# Create the moodledata directory
RUN mkdir -p /var/www/moodledata

# Set working directory
WORKDIR /var/www/html

# Set correct permissions
RUN chown -R www-data:www-data /var/www/ && \
    chmod -R 755 /var/www

# Expose port 80
EXPOSE 80