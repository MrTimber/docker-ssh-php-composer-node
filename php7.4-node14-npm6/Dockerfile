FROM debian:buster-slim

# Use non interactive mode during the build
ARG DEBIAN_FRONTEND=noninteractive

# Set locale
ENV LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8
RUN apt-get update && apt-get install -y \
    locales \
    && rm -rf /var/lib/apt/lists/*
RUN sed -i -e "s/# $LANG.*/$LANG UTF-8/" /etc/locale.gen \
    && locale-gen \
    && dpkg-reconfigure locales \
    && update-locale LANG=$LANG

# Set timezone
ARG TZ="Europe/Paris"
RUN apt-get update && apt-get install -y \
    tzdata \
    && rm -rf /var/lib/apt/lists/*
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
    && echo $TZ > /etc/timezone \
    && dpkg-reconfigure tzdata

# Install base dependencies
RUN apt-get update && apt-get install -y \
    apt-transport-https \
    build-essential \
    ca-certificates \
    curl \
    git \
    iputils-ping \
    lsb-release \
    openssh-client \
    rsync \
    wget \
    zip unzip \
    && rm -rf /var/lib/apt/lists/*

# Install PHP 7.4
RUN wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
RUN echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list
RUN apt-get update && apt-get install -y \
    php7.4 \
    php7.4-cli \
    php7.4-common \
    php7.4-bcmath \
    php7.4-mbstring \
    php7.4-curl \
    php7.4-pdo \
    php7.4-mysql \
    php7.4-zip \
    php7.4-bz2 \
    php7.4-json \
    php7.4-xml \
    php7.4-intl \
    php7.4-gd \
    php7.4-imagick \
    && rm -rf /var/lib/apt/lists/*

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer \
    && chmod +x /usr/local/bin/composer \
    && composer self-update

# Install Node.js 14, npm 6 and gulp-cli
RUN curl -fsSL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get update && apt-get install -y nodejs \
    && rm -rf /var/lib/apt/lists/*
RUN npm install npm@6 -g
RUN npm install -g gulp-cli

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