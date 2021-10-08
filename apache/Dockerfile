FROM php:7.4-apache

LABEL author="Ernest KOUASSI <ernestkouassi02@gmail.com>"

# Install System Dependencies

RUN apt-get update \
	&& DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
	software-properties-common \
	&& apt-get update \
	&& DEBIAN_FRONTEND=noninteractive apt-get install -y \
	nodejs \
	libfreetype6-dev \
	libicu-dev \
    libssl-dev \
	libjpeg62-turbo-dev \
	libmcrypt-dev \
	libedit-dev \
	libedit2 \
	libxslt1-dev \
	libzip-dev \
	apt-utils \
	gnupg \
	zlib1g-dev \
	libpng-dev \
	libjpeg-dev \
	vim \
	wget \
	curl \
	lynx \
	psmisc \
	unzip \
	tar \
	cron \
	bash-completion \
	&& apt-get clean


# Magento dependencies
RUN apt-get update && apt-get install -y \
            libfreetype6-dev \
            libjpeg62-turbo-dev \
            libpng-dev \
  && docker-php-ext-configure \
  	gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/; \
    docker-php-ext-install \
  	opcache \
  	bcmath \
  	gd \
  	intl \
  	mysqli \
  	pdo_mysql \
  	soap \
  	xsl \
  	zip \
  	sockets

# Install oAuth
RUN apt-get update \
  	&& apt-get install -y \
  	libpcre3 \
  	libpcre3-dev \
  	&& pecl install oauth \
  	&& echo "extension=oauth.so" > /usr/local/etc/php/conf.d/docker-php-ext-oauth.ini


# Install nodeJS, npm
CMD wget -O- -q https://deb.nodesource.com/setup_14.x | sudo -E bash - \
  	&& apt-get install -y nodejs build-essential \
    && npm i -g grunt-cli

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

# Configuring system
ADD /.docker/php.ini /usr/local/etc/php/php.ini
ADD /.docker/magento.conf /etc/apache2/sites-available/magento.conf
COPY /.docker/* /usr/local/bin/
COPY /.docker/* /var/www/
RUN chmod +x /usr/local/bin/*
RUN ln -s /etc/apache2/sites-available/magento.conf /etc/apache2/sites-enabled/magento.conf

# RUN curl -o /etc/bash_completion.d/m2install-bash-completion https://raw.githubusercontent.com/yvoronoy/m2install/master/m2install-bash-completion
# RUN curl -O /etc/bash_completion.d/n98-magerun2.phar.bash https://raw.githubusercontent.com/netz98/n98-magerun2/master/res/autocompletion/bash/n98-magerun2.phar.bash
# RUN echo "source /etc/bash_completion" >> root/.bashrc
# RUN echo "source /etc/bash_completion" >> /var/www/.bashrc

RUN chmod 777 -Rf /var/www/ /var/www/.* \
	&& chown -Rf www-data:www-data /var/www /var/www/.* \
	&& usermod -u 1000 www-data \
	&& chsh -s /bin/bash www-data\
	&& a2enmod rewrite \
	&& a2enmod headers

VOLUME /var/www/html
WORKDIR /var/www/html