FROM php:8.1.0-fpm

ENV TZ=America/Sao_Paulo

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN rm /etc/apt/preferences.d/no-debian-php && \
    apt-get update -yqq && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
    wget \
    curl \
    git \
    unzip \
    zip \
    openssl \
    libpq-dev \
    libaio1 \
    libaio-dev \
    zlib1g-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    supervisor && \
    rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure pgsql --with-pgsql=/usr/local/pgsql && \
    docker-php-ext-install \
    gd \
    zip \
    mbstring \
    pcntl \
    soap \
    simplexml \
    pgsql \
    pdo_pgsql && \
    pecl install redis && \
    docker-php-ext-enable redis

# Composer
RUN curl -L -C - --progress-bar -o /usr/local/bin/composer https://getcomposer.org/composer.phar && \
    chmod 755 /usr/local/bin/composer && \
    useradd -m devs && \
    usermod -u 1000 devs

RUN composer self-update

COPY --chown=devs:www-data . /var/www

RUN chown -R www-data /var/www

USER devs

EXPOSE 9000

CMD ["php-fpm"]
