# PHP Version Argument
ARG PHP_VERSION=${PHP_VERSION}
ARG APT_PACKAGES=${APT_PACKAGES}
ARG PROJECT_DIR=${PROJECT_DIR}
ARG DB_DRIVER=${DB_DRIVER}

# Base image with PHP
FROM php:${PHP_VERSION}-fpm

RUN echo "Installing PHP-${PHP_VERSION}, with Nginx, Composer, Node.js, and DB Driver: $DB_DRIVER" \
    && echo "Project Directory set to: ${PROJECT_DIR}" \
    && echo "Additional APT Packages: ${APT_PACKAGES}"

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# --------------------------
# Install Node.js + npm (LTS)
# --------------------------
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - && apt-get update && apt-get install -y nodejs && npm install -g npm@latest

# Install NGINX and configure.
RUN apt-get update && apt-get install -y nginx git zip

# Install DB Driver Extensions.
RUN set -e; \
    echo "DB_DRIVER=$DB_DRIVER"; \
    if [ "$DB_DRIVER" = "sqlite" ]; then \
        npm install sqlite3; \
        docker-php-ext-install pdo_sqlite; \
    elif [ "$DB_DRIVER" = "pgsql" ]; then \
        docker-php-ext-install pdo_pgsql pgsql; \
    elif [ "$DB_DRIVER" = "mysql" ]; then \
        docker-php-ext-install pdo_mysql; \
    else \
        docker-php-ext-install pdo_mysql; \
    fi

# Install additional APT packages if specified.
RUN if [ -n "$APT_PACKAGES" ]; then \
      apt-get update && \
      apt-get install -y ${APT_PACKAGES} && \
      apt-get clean && rm -rf /var/lib/apt/lists/*; \
    fi

# Copy custom NGINX configuration file.
COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
# COPY default.conf /etc/nginx/sites-available/default

# Set the working directory.
WORKDIR /var/www/html

# Expose ports for both PHP and NGINX
EXPOSE 80

# Start both NGINX and PHP-FPM
CMD service nginx start && php-fpm
