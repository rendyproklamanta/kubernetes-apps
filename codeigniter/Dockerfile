# Build your PHP application with PHP-FPM
FROM randyproxz/php8.1-fpm-alpine

ARG WEB_PATH=/usr/share/nginx/html
WORKDIR ${WEB_PATH}

# Install mandatory
RUN apk update
RUN apk add --no-cache nginx supervisor tzdata
RUN ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime

# Create the cache nginx
RUN mkdir -p /var/cache/nginx/fastcgi
RUN chown -R nginx:nginx /var/cache/nginx

# Create log file
RUN mkdir -p /var/log/php
RUN mkdir -p /var/log/nginx

COPY ./source ${WEB_PATH}
COPY --from=composer /app/vendor/ ${WEB_PATH}/vendor/
COPY ./conf/production/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY ./conf/nginx.conf /etc/nginx/nginx.conf
COPY ./conf/production/default.conf /etc/nginx/conf.d/default.conf 
COPY ./conf/production/php.ini /usr/local/etc/php/php.ini
COPY ./conf/docker.conf /usr/local/etc/php-fpm.d/docker.conf

# Composer install packages
RUN composer install --working-dir=${WEB_PATH} \
   --optimize-autoloader \
   --ignore-platform-reqs \
   --prefer-dist \
   --no-interaction \
   --no-plugins \
   --no-scripts \
   --no-dev

# Run command
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf", "-n"]