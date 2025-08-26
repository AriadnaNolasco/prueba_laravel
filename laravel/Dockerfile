# Imagen base oficial de PHP con extensiones necesarias
FROM php:8.2-cli

# Instalar dependencias del sistema y extensiones de PHP
RUN apt-get update && apt-get install -y \
    unzip \
    git \
    libpq-dev \
    libonig-dev \
    libzip-dev \
    zip \
    && docker-php-ext-install pdo pdo_mysql mbstring zip

# Instalar Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Copiar archivos del proyecto
WORKDIR /app
COPY ./laravel .

RUN mkdir -p bootstrap/cache \
    && chmod -R 775 bootstrap/cache \
    && chown -R www-data:www-data bootstrap/cache

# Instalar dependencias de Laravel
RUN composer install --no-dev --optimize-autoloader

# Generar caches de Laravel
RUN php artisan config:cache && \
    php artisan route:cache && \
    php artisan view:cache

# Puerto que Render usará
EXPOSE 10000

# Comando para iniciar Laravel
CMD php artisan serve --host 0.0.0.0 --port 10000


