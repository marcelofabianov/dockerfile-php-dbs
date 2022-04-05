FROM php:8.1.0-fpm

ENV TZ=America/Sao_Paulo

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Gravando o repositório Symfony CLI 
RUN echo 'deb [trusted=yes] https://repo.symfony.com/apt/ /' | tee /etc/apt/sources.list.d/symfony-cli.list

# Dependencias
RUN rm /etc/apt/preferences.d/no-debian-php && \
    apt-get update -yqq && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
    git \
    wget \
    curl \
    unzip \
    zip \
    openssl \
    libpq-dev \
    libaio1 \
    libaio-dev \
    zlib1g-dev \
    libonig-dev \
    libzip-dev \
    libicu-dev \
    libmemcached-dev \
    symfony-cli && \
    rm -rf /var/lib/apt/lists/*

# PHP Extensões
RUN docker-php-ext-configure pgsql --with-pgsql=/usr/local/pgsql && \
    docker-php-ext-configure intl && \
      pecl install redis && \
      pecl install apcu && \
    docker-php-ext-install \
      intl \
      zip \
      mbstring \
      pcntl \
      sockets \
      exif \
      pgsql \
      pdo_pgsql && \
    docker-php-ext-enable \
      redis \
      opcache \
      apcu && \
    pecl clear-cache

# Composer
RUN curl -L -C - --progress-bar -o /usr/local/bin/composer https://getcomposer.org/composer.phar && \
    chmod 755 /usr/local/bin/composer && \
    useradd -m devs && \
    usermod -u 1000 devs

RUN composer self-update

RUN echo "short_open_tag = Off" >> /usr/local/etc/php/conf.d/short_open_tag.ini

# Permissões e usuário
COPY --chown=devs:www-data . /var/www

RUN chown -R www-data /var/www

USER devs

EXPOSE 9000

CMD ["php-fpm"]
