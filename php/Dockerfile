FROM ubuntu:20.04

ARG PHP_VERSION=7.4
ARG USERNAME=app
ARG USER_UID=1000
ARG USER_GID=${USER_UID}
ARG DEBIAN_FRONTEND=noninteractive

LABEL maintainer="Madalin Ignisca <hi@madalin.me>"
LABEL description="PHP image to run PHPFPM or PHP Workers, suitable for Laravel, but not only"
LABEL repo="https://github.com/madalinignisca/dockerimages"

ENV PRECISION=14
ENV OUTPUT_BUFFERING=4096
ENV MAX_EXECUTION_TIME=30
ENV MAX_INPUT_TIME=60
ENV MEMORY_LIMIT=128M
ENV ERROR_REPORTING="E_ALL & ~E_DEPRECATED & ~E_STRICT"
ENV DISPLAY_ERRORS=Off
ENV LOG_ERROR_MAX_LEN=1024
ENV POST_MAX_SIZE=8M
ENV UPLOAD_MAX_FILESIZE=2M
ENV MAX_FILE_UPLOADS=20
ENV DATE_TIMEZONE=UTC
ENV OPCACHE_MEMORY_CONSUMPTION=128
ENV OPCACHE_INTERNED_STRINGS_BUFFER=8
ENV OPCACHE_MAX_ACCELERATED_FILES=10000
ENV OPCACHE_USE_CWD=1
ENV OPCACHE_VALIDATE_TIMESTAMPS=1
ENV OPCACHE_REVALIDATE_FREQ=2
ENV OPCACHE_REVALIDATE_PATH=0
ENV OPCACHE_SAVE_COMMENTS=1
ENV OPCACHE_ENABLE_FILE_OVERRIDE=0
ENV OPCACHE_PRELOAD=
ENV OPCACHE_PRELOAD_USER=

ENV PM=dynamic
ENV PM_START_SERVERS=2
ENV PM_MIN_SPARE_SERVERS=1
ENV PM_MAX_SPARE_SERVERS=3
ENV PM_MAX_CHILDREN=4
ENV PM_PROCESS_IDLE_TIMEOUT=10s
ENV ACCESS_FORMAT="%R - %u %t \"%m %r%Q%q\" %s %f %{mili}d %{kilo}M %C%%"
ENV REQUEST_SLOWLOG_TIMEOUT=0
ENV REQUEST_SLOWLOG_TRACE_DEPTH=20
ENV REQUEST_TERMINATE_TIMEOUT=0


RUN groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd --create-home --shell /bin/bash --uid ${USER_UID} --gid ${USER_GID} ${USERNAME}

RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install --no-install-recommends -y \
      gnupg \
      software-properties-common \
    && LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php

RUN apt-get install --no-install-recommends -y \
    php${PHP_VERSION}-apcu \
    php${PHP_VERSION}-bcmath \
    php${PHP_VERSION}-cli \
    php${PHP_VERSION}-curl \
    php${PHP_VERSION}-fpm \
    php${PHP_VERSION}-gd \
    php${PHP_VERSION}-intl \
    php${PHP_VERSION}-mbstring \
    php${PHP_VERSION}-memcached \
    php${PHP_VERSION}-mysql \
    php${PHP_VERSION}-pgsql \
    php${PHP_VERSION}-redis \
    php${PHP_VERSION}-xml \
    php${PHP_VERSION}-zip
    
RUN ln -s /usr/sbin/php-fpm${PHP_VERSION} /usr/sbin/php-fpm

COPY php.ini /etc/php/${PHP_VERSION}/cli/php.ini
COPY php.ini /etc/php/${PHP_VERSION}/fpm/php.ini
COPY php.ini /etc/php/${PHP_VERSION}/cli/php.ini
COPY php-fpm.conf /etc/php/${PHP_VERSION}/fpm/php-fpm.conf

STOPSIGNAL SIGQUIT

EXPOSE 9000
EXPOSE 9001
CMD ["php-fpm"]
