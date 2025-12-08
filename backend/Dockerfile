FROM php:8.4-fpm

WORKDIR /var/www/html

RUN apt-get update && apt-get install -y \
    git curl libpng-dev libonig-dev libxml2-dev zip unzip libpq-dev libzip-dev \
    npm nodejs \
    && docker-php-ext-install pdo pdo_pgsql mbstring exif pcntl bcmath gd zip

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Create Laravel if composer.json does not exist
RUN if [ ! -f composer.json ]; then \
      composer create-project laravel/laravel . ; \
    fi

# Install dependencies
RUN composer install --prefer-dist --no-dev --no-scripts --no-autoloader

COPY . /var/www/html

RUN composer install --prefer-dist --no-dev --no-scripts --no-autoloader
RUN npm install

EXPOSE 9000
CMD ["php-fpm"]
