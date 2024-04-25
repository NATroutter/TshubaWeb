FROM ubuntu:jammy
ARG DEBIAN_FRONTEND=noninteractive

RUN mkdir -p /var/run/php/

RUN mkdir -p /var/run/php-fpm
RUN chown -R www-data:www-data /var/run/php-fpm

RUN apt-get update -y
RUN apt-get install --no-install-recommends -y php8.1-fpm php-mysql php-redis php-curl nginx

RUN apt install -y supervisor

COPY ./config/php.ini /etc/php/8.1/fpm/php.ini
COPY ./config/php-fpm.conf /etc/php/8.1/fpm/php-fpm.conf
COPY ./config/www.conf /etc/php/8.1/fpm/pool.d/www.conf
COPY ./config/nginx.conf /etc/nginx/nginx.conf
COPY ./config/supervisord.conf /etc/supervisor/supervisord.conf

RUN rm -f /etc/nginx/sites-enabled/default
COPY ./config/vhosts/*.conf /etc/nginx/sites-enabled/

RUN rm -rf /var/www/html/
COPY --chown=www-data:www-data --chmod=755 ./hosts/ /var/www/

COPY ./ssl/ /etc/ssl/

RUN mkdir /var/lib/php/session
RUN chown www-data:www-data /var/lib/php/session
RUN chmod -R 0700 /var/lib/php/session

CMD /usr/bin/supervisord -n -c /etc/supervisor/supervisord.conf