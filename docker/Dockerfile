FROM php:7.0-cli

MAINTAINER Tobias Munk tobias@diemeisterei.de

ENV COMPOSER_ALLOW_SUPERUSER=1 \
    PATH=/repo:${PATH}

# Install required system packages
RUN apt-get update && \
    apt-get -y install --no-install-recommends \
        git \
        zlib1g-dev \
        libssl-dev && \

    # Install php extensions
    docker-php-ext-install \
        bcmath \
        zip && \

    # Install pecl extensions
    pecl install mongodb xdebug && \
    docker-php-ext-enable mongodb && \
    docker-php-ext-enable xdebug && \

    apt-get clean && \

    # Configure php
    echo "date.timezone = UTC" >> /usr/local/etc/php/php.ini && \

    # Install composer
    curl -sS https://getcomposer.org/installer | php -- \
            --filename=composer \
            --install-dir=/usr/local/bin && \
    composer global require --optimize-autoloader \
        "hirak/prestissimo" && \

    # Add source-code
    git clone -b 2.2 --single-branch https://github.com/herloct/Codeception.git /repo && \
    rm -rf /repo/.git && \

    # Install vendor
    composer install --prefer-dist --optimize-autoloader --working-dir /repo && \
    composer clear-cache && \

    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENTRYPOINT ["codecept"]

# Prepare host-volume working directory
RUN mkdir /project
VOLUME /project
WORKDIR /project
