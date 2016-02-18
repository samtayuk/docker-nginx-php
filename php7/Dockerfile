FROM ubuntu:trusty
MAINTAINER Samuel Taylor "samtaylor.uk@gmail.com"

ENV NGINX_VERSION 1.9.10-1+trusty0

# Install wget and install Nginx
RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys C300EE8C \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E5267A6C \
    && echo "deb http://ppa.launchpad.net/nginx/development/ubuntu trusty main" >> /etc/apt/sources.list \
    && echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu trusty main" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y -q --no-install-recommends \ 
      ca-certificates \
      nginx=${NGINX_VERSION} \
      gettext-base \
      wget \
      curl \
      git \
      cron \
      php7.0-curl \
      php7.0-fpm \
      php7.0-gd \
      php7.0-bz2 \
      php7.0-mysql \
      php7.0-mcrypt \
      php7.0-sqlite \
      php7.0-odbc \
      php7.0-opcache \
      php7.0-pgsql  \
      php-apcu \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /etc/nginx/sites-enabled/default \
    && mkdir -p /run/php

# Configure Nginx and apply fix for long server names and Pass all docker environment
RUN echo "daemon off;" >> /etc/nginx/nginx.conf \
  && sed -i 's/^http {/&\n    server_names_hash_bucket_size 64;/g' /etc/nginx/nginx.conf \
  && sed -i '/^;clear_env = no/s/^;//' /etc/php/7.0/fpm/pool.d/www.conf


# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log


# Install Forego
RUN wget -P /usr/local/bin https://godist.herokuapp.com/projects/ddollar/forego/releases/current/linux-amd64/forego \
  && chmod u+x /usr/local/bin/forego


COPY Procfile /app/
WORKDIR /app/

COPY index.php /app/www/
COPY default.conf /etc/nginx/conf.d/
COPY php.ini /etc/php/7.0/fpm/conf.d/40-custom.ini

EXPOSE 80 443

CMD ["/usr/local/bin/forego", "start", "-r"]
