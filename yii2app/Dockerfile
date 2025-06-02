FROM php:8.2-apache

# Install system packages and PHP extensions
RUN apt-get update && apt-get install -y \
    git unzip libpng-dev libjpeg-dev libonig-dev libxml2-dev zip curl \
    && docker-php-ext-install pdo pdo_mysql mbstring gd xml

# Enable Apache rewrite module
RUN a2enmod rewrite

# Set Apache DocumentRoot to /var/www/html/web
ENV APACHE_DOCUMENT_ROOT /var/www/html/web

# Update Apache config to use the new DocumentRoot
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/000-default.conf

# Allow override in web directory
RUN echo '<Directory "${APACHE_DOCUMENT_ROOT}">\n\
    AllowOverride All\n\
    Require all granted\n\
</Directory>' > /etc/apache2/conf-available/allow-override.conf && \
    a2enconf allow-override

# Copy Yii2 app files
COPY . /var/www/html

# Set permissions
RUN chown -R www-data:www-data /var/www/html && \
    chmod -R 755 /var/www/html

