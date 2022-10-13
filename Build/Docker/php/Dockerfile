ARG php_version

FROM php:${php_version}

RUN apt update
RUN apt install -y --no-install-recommends \
        unzip \
        iputils-ping \
        imagemagick \
        ghostscript \
        default-mysql-client \
        wget \
        libxml2-dev libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libpq-dev \
        libzip-dev \
        zlib1g-dev \
        imagemagick \
        ghostscript \
        graphicsmagick \
        supervisor && \
    docker-php-ext-configure gd --with-libdir=/usr/include/ --with-jpeg --with-freetype && \
    docker-php-ext-install -j$(nproc) mysqli soap gd zip opcache intl pgsql pdo_pgsql && \
    echo 'always_populate_raw_post_data = -1\nmax_execution_time = 240\nmax_input_vars = 1500\nupload_max_filesize = 32M\npost_max_size = 32M' > /usr/local/etc/php/conf.d/typo3.ini

RUN apt clean && \
    apt -y purge \
        libxml2-dev libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libzip-dev \
        zlib1g-dev && \
    rm -rf /var/lib/apt/lists/* /usr/src/*


ENV COMPOSER_BINARY=/usr/local/bin/composer \
    COMPOSER_HOME=/usr/local/composer
ENV PATH $PATH:$COMPOSER_HOME

RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar $COMPOSER_BINARY && \
    chmod +x $COMPOSER_BINARY

COPY php.ini /etc/php8/fpm/php.ini
COPY php.ini /etc/php8/cli/php.ini

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /var/www/html

EXPOSE 9000
CMD ["php-fpm", "-F", "-c", "/etc/php8/fpm"]