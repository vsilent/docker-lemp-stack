# PHP 7.2
FROM php:7.2-fpm

# upgrade the container
RUN apt-get update && \
    apt-get upgrade -y

#RUN curl -sL https://deb.nodesource.com/setup_6.x | bash -

RUN apt-get install -y ap-utils curl git nano zlib1g-dev nodejs \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    wget \
    libldb-dev \
    vim \
    unzip \
    libpq-dev \
    libgd2-dev \
    libjpeg-dev \
    libgif-dev \
    libmcrypt-dev \
    libmagickwand-dev \
    && docker-php-ext-install -j$(nproc) zip bz2 mbstring pdo pdo_mysql mysqli sockets pcntl \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && pecl install redis \
    && pecl install imagick \
    && docker-php-ext-enable redis imagick

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

ARG XDEBUG_INI=/usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN echo "xdebug.default_enable = off" >> ${XDEBUG_INI} \
    && echo "xdebug.remote_enable = on" >> ${XDEBUG_INI} \
    && echo "xdebug.remote_autostart = off" >> ${XDEBUG_INI} \
    && echo "xdebug.remote_connect_back = off" >> ${XDEBUG_INI} \
    && echo "xdebug.remote_port = 9001" >> ${XDEBUG_INI} \
    && echo "xdebug.remote_host = 10.254.254.254" >> ${XDEBUG_INI} \
    && echo "xdebug.remote_log = /var/www/docker/php/xdebug.log" >> ${XDEBUG_INI}

# Install composer
# ENV COMPOSER_ALLOW_SUPERUSER=1
# RUN curl -sS https://getcomposer.org/installer | php && \
#     mv composer.phar /usr/local/bin/composer && \
#     printf "\nPATH=\"~/.composer/vendor/bin:\$PATH\"\n" | tee -a ~/.bashrc

# Composer plugins
# RUN composer global require "hirak/prestissimo"
# RUN composer global require "laravel/envoy"
# RUN composer global require "laravel/installer"

# Install Laravel
# RUN ln -s /var/www/html/artisan /usr/local/bin/ \
#     && ln -s /vendor/bin/phpunit /usr/local/bin/

# Start php-fpm server
CMD ["php-fpm"]
