# PHP Version Argument
ARG PHP_VERSION=${PHP_VERSION}
ARG PROJECT_DIR=${PROJECT_DIR}

# Base image with PHP
FROM php:${PHP_VERSION}-fpm

# Install additional dependencies (e.g., extensions, composer)
RUN apt-get update && apt-get install -y \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    unzip \
    git \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip pdo pdo_mysql gd

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# --------------------------
# Install Node.js + npm (LTS)
# --------------------------
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - && apt-get update && apt-get install -y nodejs && npm install -g npm@latest

# Set working directory to /var/www
WORKDIR /var/www/html

RUN if [ -n ${APT_PACKAGES} ]; then \
      apt-get update && \
      apt-get install -y ${APT_PACKAGES} && \
      apt-get clean && rm -rf /var/lib/apt/lists/*; \
    fi

# Copy apps into the container
COPY ${PROJECT_DIR} /var/www/html

RUN chmod -R 755 /var/www/html && chown -R www-data:www-data /var/www/html

# Expose port 9000 and start php-fpm
EXPOSE 9000
CMD ["php-fpm"]
