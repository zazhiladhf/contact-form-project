FROM php:8.3.12-fpm

WORKDIR /var/www/html

# System dependencies (hati-hati supaya tidak crash di ARM)
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    libzip-dev \
    libpq-dev \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip

# Install Composer (from separate multi-arch container)
COPY --from=composer:2.7 /usr/bin/composer /usr/bin/composer

# Copy Laravel source code
COPY . /var/www/html

# Laravel setup
RUN cp .env.example .env \
 && chown -R www-data:www-data /var/www/html \
 && chmod -R 775 /var/www/html \
 && composer install --no-dev --optimize-autoloader \
 && php artisan key:generate

USER www-data

EXPOSE 9000

CMD ["php-fpm"]
