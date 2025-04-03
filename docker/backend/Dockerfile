FROM php:8.2-fpm

# System-Abhängigkeiten installieren
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    default-jdk \
    && rm -rf /var/lib/apt/lists/*

# PHP-Extensionen installieren
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Composer installieren
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Arbeitsverzeichnis setzen
WORKDIR /var/www

# Composer-Abhängigkeiten kopieren
COPY backend/composer.json backend/composer.lock ./

# Composer-Abhängigkeiten installieren
RUN composer install --no-scripts --no-autoloader

# Anwendungscode kopieren
COPY backend/ .

# Composer-Autoloader optimieren
RUN composer dump-autoload --optimize

# Berechtigungen setzen
RUN chown -R www-data:www-data /var/www

# PHP-FPM-Konfiguration
COPY docker/backend/php-fpm.conf /usr/local/etc/php-fpm.d/www.conf
COPY docker/backend/php.ini /usr/local/etc/php/conf.d/custom.ini

EXPOSE 9000

CMD ["php-fpm"] 