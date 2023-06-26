ARG PHP_VERSION
ARG APPLICATION_NAME=application.com.br
ARG APP_DIR=/var/www/${APPLICATION_NAME}
ARG REDIS_LIB_VERSION=5.3.7

FROM php:${PHP_VERSION}

### apt-utils é um extensão de recursos do gerenciador de pacotes APT
RUN apt-get update -y && apt-get install -y --no-install-recommends \
    apt-utils \
    supervisor

# dependências recomendadas de desenvolvido para ambiente linux
RUN apt-get update && apt-get install -y \
    zlib1g-dev \
    libzip-dev \
    unzip \
    libpng-dev \
    libpq-dev \
    libxml2-dev

RUN docker-php-ext-install mysqli pdo pdo_mysql pdo_pgsql pgsql session xml

# habilita instalação do Redis
RUN pecl install redis-${REDIS_LIB_VERSION} \
    && docker-php-ext-enable redis

RUN docker-php-ext-install zip iconv simplexml pcntl gd fileinfo

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY ./docker/supervisord/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY ./docker/supervisord/supervisord.conf /etc/supervisord.d/

COPY ./docker/php/extra-php.ini "$PHP_INI_DIR/99_extra.ini"
COPY ./docker/php/extra-php-fpm.conf /etc/php8/php-fpm.d/www.conf

WORKDIR $APP_DIR
RUN cd $APP_DIR
RUN chown www-data:www-data $APP_DIR

COPY --chown=www-data:www-data ./${APPLICATION_NAME}/app .
RUN rm -rf vendor
RUN composer install --no-interaction
RUN php artisan storage:link && php artisan key:generate --no-interaction

# Instalar o Node.js 18
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
RUN apt-get update && apt-get install -y nodejs

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
