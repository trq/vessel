FROM ubuntu:16.04

MAINTAINER Chris Fidao

RUN echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu xenial main" > /etc/apt/sources.list.d/ppa_ondrej_php.list \
    && echo "deb http://ppa.launchpad.net/nginx/development/ubuntu xenial main" > /etc/apt/sources.list.d/ppa_nginx_mainline.list \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E5267A6C \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys C300EE8C \
    && apt-get update \
    && apt-get install -y curl zip unzip git supervisor sqlite3 \
    && apt-get install -y nginx php7.1-fpm php7.1-cli php7.1-mcrypt php7.1-gd php7.1-mysql \
       php7.1-pgsql php7.1-imap php-memcached php7.1-mbstring php7.1-xml php7.1-curl \
       php7.1-imagick php7.1-zip php7.1-bcmath php7.1-sqlite3 php7.1-xdebug \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && mkdir /run/php \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && echo "daemon off;" >> /etc/nginx/nginx.conf

RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log

COPY h5bp /etc/nginx/h5bp
COPY default /etc/nginx/sites-available/default
COPY php-fpm.conf /etc/php/7.1/fpm/php-fpm.conf
COPY xdebug.ini /etc/php/7.1/mods-available/xdebug.ini

EXPOSE 80

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY start-container /usr/local/bin/start-container
RUN chmod +x usr/local/bin/start-container

ENTRYPOINT ["start-container"]

RUN chown -R www-data: /var/www/html
