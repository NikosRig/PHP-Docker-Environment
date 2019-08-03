FROM php:7.2-fpm

ARG UID
ARG GID

RUN apt-get update && apt-get install -y --no-install-recommends curl libmcrypt-dev libc-client-dev libkrb5-dev git \
    zip unzip mariadb-client \
    && rm -r /var/lib/apt/lists/* \
# Change default phpfpm user and group
    && sed -i -e"s/^user\s=\swww-data/user = develuser/" /usr/local/etc/php-fpm.d/www.conf \
    && sed -i -e"s/^group\s=\swww-data/group = develgroup/" /usr/local/etc/php-fpm.d/www.conf \
# Create group
    && groupadd develgroup -g $GID \
# Create user
    && useradd -ms /bin/bash develuser -u $UID -g $GID \
# Install required modules
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install imap \
    && pecl install mcrypt-1.0.1 \
# Xdebug install
    && pecl install xdebug && docker-php-ext-enable mcrypt xdebug \
# Change ownership and permissions to project folder
    && chown -R develuser:develgroup /home/develuser && chmod -R 775 /home/develuser

# Replace php.ini
COPY php.ini /usr/local/etc/php/

# Copy xdebug configuration
COPY xdebug.ini /usr/local/etc/php/conf.d/

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- \
--install-dir=/usr/bin --filename=composer && chmod +x /usr/bin/composer

WORKDIR /home/develuser
