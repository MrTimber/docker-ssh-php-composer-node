FROM ubuntu:18.04

# Set default file encoding to UTF-8
ENV LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    LANGUAGE=C.UTF-8

# Set timezone to Europe/Paris
ARG TZ="Europe/Paris"
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Use non interactive mode during the build
ARG DEBIAN_FRONTEND=noninteractive

# Install base dependencies
RUN apt-get update
RUN apt-get install -y \
    sudo \
    autoconf \
    autogen \
    language-pack-en-base \
    wget \
    zip \
    unzip \
    curl \
    rsync \
    ssh \
    openssh-client \
    git \
    build-essential \
    apt-utils \
    software-properties-common \
    nasm \
    libjpeg-dev \
    libpng-dev \
    libpng16-16

# PHP
RUN LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php && apt-get update && apt-get install -y php7.3
RUN apt-get install -y \
    php7.3-curl \
    php7.3-gd \
    php7.3-dev \
    php7.3-xml \
    php7.3-bcmath \
    php7.3-mysql \
    php7.3-pgsql \
    php7.3-mbstring \
    php7.3-zip \
    php7.3-bz2 \
    php7.3-sqlite \
    php7.3-soap \
    php7.3-json \
    php7.3-intl \
    php7.3-imap \
    php7.3-imagick \
    php-memcached
RUN command -v php

# Composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer && \
    chmod +x /usr/local/bin/composer && \
    composer self-update
RUN command -v composer

# Node.js 14 and npm 6
RUN curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
RUN sudo apt-get install -y nodejs
RUN npm install npm@6 -g
RUN command -v node
RUN command -v npm

# Display versions installed
RUN php -v
RUN composer --version
RUN node -v
RUN npm -v

# Create directories
RUN mkdir -p ~/.ssh
RUN touch ~/.ssh_config
RUN mkdir -p /opt/atlassian/bitbucketci/agent/build

# Customize root bash prompt
RUN sed -i '/[ -z \"PS1\" ] && return/a\\ncase $- in\n*i*) ;;\n*) return;;\nesac' /root/.bashrc

# Creates pipelines user
RUN useradd --create-home --shell /bin/bash --uid 1000 pipelines

# Set working directory and entrypoint
WORKDIR /opt/atlassian/bitbucketci/agent/build
ENTRYPOINT /bin/bash