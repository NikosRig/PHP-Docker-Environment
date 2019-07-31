FROM php:7.2-fpm

ARG UID
ARG GID

RUN apt-get update && apt-get install -y --no-install-recommends curl libmcrypt-dev libc-client-dev libkrb5-dev git \
    zip unzip mariadb-client iputils-ping \
    && rm -r /var/lib/apt/lists/* \
# Change default phpfpm user and group
    && sed -i -e"s/^user\s=\swww-data/user = develuser/" /usr/local/etc/php-fpm.d/www.conf \
    && sed -i -e"s/^group\s=\swww-data/group = develgroup/" /usr/local/etc/php-fpm.d/www.conf \
# Create group
    && groupadd develgroup -g $GID \
# Create user
    && useradd -ms /bin/bash develuser -u $UID -g $GID \
# Install required modules
    && docker-php-ext-install pdo_mysql -j$(nproc) iconv \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install imap \
    && pecl install mcrypt-1.0.2 \
# Xdebug install and configuration
    && pecl install xdebug-2.6.0 && docker-php-ext-enable xdebug  mcrypt \
    && echo "xdebug.default_enable=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_port=10000" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_handler=dbgp" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug_remote_host=localhost" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_autostart=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.idekey=PHPSTORM" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# Change ownership and permissions to project folder
    && chown -R develuser:develgroup /home/develuser && chmod -R 775 /home/develuser


# Replace php.ini
COPY php.ini /usr/local/etc/php/

#COPY xdebug.ini /usr/local/etc/php/conf.d/

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- \
--install-dir=/usr/bin --filename=composer && chmod +x /usr/bin/composer

WORKDIR /home/develuser
