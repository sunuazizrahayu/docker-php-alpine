FROM alpine:3.5

RUN apk add --update \
	curl \
	nginx \
	\
	php7 php7-fpm \
	php7-opcache php7-session php7-mysqli \
	php7-phar php7-iconv php7-json php7-openssl \
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
RUN ln -s /usr/bin/php7 /usr/bin/php
RUN ln -s /usr/sbin/php-fpm7 /usr/sbin/php-fpm

# Configure PHP-FPM
COPY config/fpm-pool.conf /etc/php7/php-fpm.d/www.conf


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
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

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