FROM alpine:3.5

ENV TIMEZONE Europe/Paris

# RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/main"  >> /etc/apk/repositories

RUN apk update && \
    apk upgrade && \
    apk add --update tzdata && \
    cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
    echo "${TIMEZONE}" > /etc/timezone && \
    apk upgrade && \
    apk add --update tzdata

RUN apk --no-cache \
    add php7 \
    php7-fpm \
    php7-mysqli \
    php7-pdo_mysql \
    php7-sqlite3 \
    php7-pdo_pgsql \
    php7-bcmath \
    php7-odbc \
    php7-pdo_mysql \
    php7-pdo_sqlite  \
    php7-json \
    php7-openssl \
    php7-curl \
    php7-zlib \
    php7-session \
    php7-xml \
    php7-phar \
    php7-intl \
    php7-dom \
    php7-xmlreader \
    php7-ctype \
    php7-mcrypt \
    php7-mbstring \
    php7-gd \
    nginx \
    supervisor \
    curl \
    --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing/

RUN ln -s /usr/bin/php7 /usr/bin/php
RUN apk del tzdata && \
    curl -sS https://getcomposer.org/installer | php7 -- --install-dir=/usr/bin --filename=composer \
#     composer global require hirak/prestissimo

RUN sed -i "s|;date.timezone =.*|date.timezone=${TIMEZONE}|" /etc/php7/php.ini

COPY config/nginx.conf /etc/nginx/nginx.conf

COPY config/fpm-pool.conf /etc/php7/php-fpm.d/custom.conf

COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN mkdir -p /app/public
WORKDIR /app

EXPOSE 80
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
