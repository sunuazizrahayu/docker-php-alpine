FROM alpine:3.6

RUN apk add --update \
	curl \
	nginx \
	\
	php5 php5-fpm \
	php5-opcache \
	php5-mysqli \
	\
	php5-phar php5-iconv php5-json php5-openssl \
	php5-zlib \
	\
	# development purpose \
	nano

# NGINX
######################################################################
# Setup NGINX App config
COPY config/nginx.app.conf /etc/nginx/conf.d/default.conf


# PHP
######################################################################
# Create symlink so programs depending on `php` still function
RUN ln -s /usr/bin/php5 /usr/bin/php
RUN ln -s /usr/bin/php-fpm5 /usr/bin/php-fpm

# Configure PHP-FPM
COPY config/php-fpm.conf /etc/php5/php-fpm.conf


# Preparation
######################################################################
# Prepare Directory
RUN mkdir -p \
	/run/nginx \
	/app

# Change PATH
WORKDIR /app


# Composer
######################################################################
# Install Composer
#RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install Composer Dependency
#COPY composer.json composer.lock ./
#RUN composer install --working-dir=. --prefer-dist --no-scripts --no-dev && rm -rf /root/.composer


# ADD Project Files
COPY htdocs/ .


# start nginx & php-fpm
COPY config/start.sh /
CMD sh /start.sh

# Configure a healthcheck to validate that everything is up & running
HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:80/fpm-ping