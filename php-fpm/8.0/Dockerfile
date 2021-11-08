FROM php:8.0-fpm-alpine

ARG XDEBUG_PORT
ARG DEV_ENV
ARG APP_DIR
ARG USER_NAME
ARG USER_ID
ARG USER_GROUP_NAME
ARG USER_GROUP_ID
#ARG GIT_USER_NAME
#ARG GIT_USER_EMAIL

ENV XDEBUG_PORT=${XDEBUG_PORT:-9000}
ENV DEV_ENV=${DEV_ENV:-"local"}
ENV APP_DIR=${APP_DIR:-"/apps"}
ENV USER_NAME=${USER_NAME:-"tarrissarh"}
ENV USER_ID=${USER_ID:-1000}
ENV USER_GROUP_NAME=${USER_GROUP_NAME:-"tarrissarh"}
ENV USER_GROUP_ID=${USER_GROUP_ID:-1000}
#ENV GIT_USER_NAME=${GIT_USER_NAME:-"Tarrissarh"}
#ENV GIT_USER_EMAIL=${GIT_USER_EMAIL:-"tarrissarh@gmail.com"}

LABEL maintainer="Tarrissarh <tarrissarh@gmail.com>"

# Install apps
RUN apk add --no-cache --update \
    util-linux \
    ca-certificates \
    openssl \
    bash \
    curl \
    curl-dev \
    wget \
    git \
    zip \
    unzip \
    tar \
    xz \
    zlib \
    zlib-dev \
    autoconf \
    imagemagick \
    imagemagick-dev \
    libmcrypt-dev \
    libltdl \
    libmemcached-libs \
    libmemcached-dev \
    cyrus-sasl-dev \
    bzip2-dev \
    enchant2-dev \
    libpng-dev \
    gmp-dev \
    imap-dev \
    icu-dev \
    openldap-dev \
    freetds-dev \
    postgresql-dev \
    aspell-dev \
    net-snmp-dev \
    libxml2-dev \
    tidyhtml-dev  \
    libxslt-dev \
    libzip-dev \
    $PHPIZE_DEPS

# Install PHP modules
# imagemagick
RUN set -xe \
    && git clone https://github.com/Imagick/imagick \
    && cd imagick \
    && git checkout master && git pull \
    && phpize && ./configure && make && make install \
    && cd .. && rm -rf imagick \
    && docker-php-ext-enable imagick

# xdebug
RUN if [ $DEV_ENV == 'local' ]; then pecl install xdebug ; fi

# memcached
RUN pecl install memcached

# mongodb
RUN pecl install mongodb

# Install default extensions
# Core, ctype, curl, date, dom, fileinfo, filter, ftp, hash, iconv, json, libxml, mbstring,
# mysqlnd, openssl, pcre, PDO, pdo_sqlite, Phar, posix, readline, Reflection, session, SimpleXML,
# sodium, SPL, sqlite3, standard, tokenizer, xml, xmlreader, xmlwriter, zlib
RUN docker-php-ext-install \
    bcmath \
    bz2 \
    calendar \
    dba \
    enchant \
    exif \
    ffi \
    gd \
    gettext \
    gmp \
    imap \
    intl \
    ldap \
    mysqli \
    opcache \
    pcntl \
    pdo_dblib \
    pdo_mysql \
    pdo_pgsql \
    pgsql \
    pspell \
    shmop \
    snmp \
    soap \
    sockets \
    sysvmsg \
    sysvsem \
    sysvshm \
    tidy \
    xsl \
    zend_test \
    zip

# PHP MODULES not working yet: oci8, odbc, pdo_firebird, pdo_oci, pdo_odbc

RUN set -x ; \
    addgroup -g $USER_GROUP_ID -S $USER_GROUP_NAME ; \
    adduser -u $USER_ID -D -S -G $USER_GROUP_NAME $USER_NAME && exit 0 ; exit 1

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN mkdir -p $APP_DIR && chown -R tarrissarh:tarrissarh $APP_DIR

WORKDIR $APP_DIR

# Base GIT commands
#RUN git config --global user.email $GIT_USER_EMAIL \
#    && git config --global user.name $GIT_USER_NAME \
#    && git config core.filemode false \
#    && git config --global core.filemode false

# Clear cache
RUN docker-php-source delete \
    && rm -rf /tmp/* /var/cache/apk/*

USER $USER_NAME

EXPOSE $XDEBUG_PORT

CMD ["php-fpm"]