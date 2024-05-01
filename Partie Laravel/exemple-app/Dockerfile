FROM webdevops/php-nginx:8.3-alpine
RUN apk add --no-cache openssl nginx oniguruma-dev libxml2-dev

RUN apk add oniguruma-dev libxml2-dev
RUN docker-php-ext-install \
bcmath \
ctype \
fileinfo \
mbstring \
pdo_mysql \
xml
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

ENV WEB_DOCUMENT_ROOT /app/public
ENV APP_ENV production
WORKDIR /app
COPY . .

RUN cp -n .env.example .env

RUN composer install --no-interaction --optimize-autoloader --no-dev
RUN php artisan key:generate

RUN php artisan config:cache

RUN php artisan view:cache

RUN chown -R application:application .


EXPOSE 443

CMD ["nginx", "-g", "daemon off;"]
