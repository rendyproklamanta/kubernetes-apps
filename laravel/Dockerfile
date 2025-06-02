# First Stage
FROM node:alpine as frontend
COPY ./source/package.json /app
COPY ./source/package-lock.json /app
RUN npm install
COPY ./source/webpack.mix.js /app
COPY ./source/resources/js/ /app/resources/js
COPY ./source/resources/sass/ /app/resources/sass
RUN npm run production

# Second Stage
FROM composer as composer
COPY ./source/composer.json /app
COPY ./source/composer.lock /app
RUN composer install \
   --optimize-autoloader \
   --ignore-platform-reqs \
   --prefer-dist \
   --no-interaction \
   --no-plugins \
   --no-scripts \
   --no-dev

# Third Stage
# Build your PHP application with PHP-FPM
FROM randyproxz/php8.1-fpm-alpine

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

ARG LARAVEL_PATH=/usr/share/nginx/html
WORKDIR ${LARAVEL_PATH}

COPY ./source ${LARAVEL_PATH}
COPY --from=composer /app/vendor/ ${LARAVEL_PATH}/vendor/
COPY --from=frontend /app/public/js/ ${LARAVEL_PATH}/public/js/
COPY --from=frontend /app/public/css/ ${LARAVEL_PATH}/public/css/
COPY --from=frontend /app/mix-manifest.json ${LARAVEL_PATH}/mix-manifest.json

RUN cd ${LARAVEL_PATH} \
      && php artisan package:discover \
      && chown www-data:www-data bootstrap/cache \
      && chown -R www-data:www-data storage/

# Copy conf files to container
COPY ./conf/production/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY ./conf/nginx.conf /etc/nginx/nginx.conf
COPY ./conf/production/default.conf /etc/nginx/conf.d/default.conf 
COPY ./conf/production/php.ini /usr/local/etc/php/php.ini
COPY ./conf/docker.conf /usr/local/etc/php-fpm.d/docker.conf

COPY entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

# Run command
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf", "-n"]