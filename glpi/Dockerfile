FROM php:8.3-apache
ARG GLPI_VERSION
RUN apt-get update && apt-get install -y --no-install-recommends \
        libicu-dev libpng-dev libjpeg-dev libfreetype6-dev libzip-dev libbz2-dev curl ca-certificates \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j"$(nproc)" mysqli gd intl bcmath zip bz2 exif opcache \
    && rm -rf /var/lib/apt/lists/*
RUN a2enmod rewrite
RUN curl -fsSL "https://github.com/glpi-project/glpi/releases/download/${GLPI_VERSION}/glpi-${GLPI_VERSION}.tgz" -o /tmp/glpi.tgz \
    && tar -xzf /tmp/glpi.tgz -C /var/www/ \
    && rm /tmp/glpi.tgz \
    && mkdir -p /var/www/glpi/marketplace \
    && chown -R www-data:www-data /var/www/glpi
COPY glpi.conf /etc/apache2/sites-available/000-default.conf
COPY glpi.ini /usr/local/etc/php/conf.d/glpi.ini
