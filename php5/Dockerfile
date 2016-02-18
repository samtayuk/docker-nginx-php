FROM samtayuk/nginx:1.9
MAINTAINER Samuel Taylor "samtaylor.uk@gmail.com"

# Install php5
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E5267A6C \
    && echo "deb http://ppa.launchpad.net/ondrej/php5-5.6/ubuntu trusty main" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y -q --no-install-recommends \ 
      cron \
      php5-curl \
      php5-fpm \
      php5-gd \
      php5-mysql \
      php5-mcrypt \
      php5-sqlite \
      php5-odbc \
      php5-pgsql  \
      php5-cli \
      php5-memcached \
      php5-apcu \
      php5-redis \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir -p /run/php \
    && sed -i '/^;clear_env = no/s/^;//' /etc/php5/fpm/pool.d/www.conf

# Install Forego
RUN wget -P /usr/local/bin https://godist.herokuapp.com/projects/ddollar/forego/releases/current/linux-amd64/forego \
  && chmod u+x /usr/local/bin/forego

COPY Procfile /app/
WORKDIR /app/

COPY index.php /app/www/
COPY default.conf /etc/nginx/conf.d/
COPY php.ini /etc/php5/fpm/conf.d/40-custom.ini

CMD ["forego", "start", "-r"]
