FROM php:7.4-fpm

ENV TZ=Europe/Moscow PHP_DIR_OWNER=root PHP_DIR_MODE=770

RUN set -ex; \
    echo "${TZ}" > /etc/timezone; \
    ln -sfn /usr/share/zoneinfo/${TZ} /etc/localtime; \
    apt-get update; \
    rm -r /var/lib/apt/lists/*; \
    docker-php-ext-install pdo_mysql bcmath; \
    apt-get purge -y --auto-remove;

COPY . /tmp/build/

RUN set -ex; \
    mkdir -p /var/php/log /var/php/tmp /var/php/sessions /var/php/upload; \
    mv /tmp/build/docker-entrypoint.sh /opt; \
    mv /tmp/build/ini/custom.ini ${PHP_INI_DIR}/conf.d; \
    rm -rf /tmp/build; \
    chown ${PHP_DIR_OWNER}:www-data /var/php/*; \
    chmod ${PHP_DIR_MODE} /var/php/*;

VOLUME ["/var/www", "/var/php"]

WORKDIR /var/www

ENTRYPOINT ["/opt/docker-entrypoint.sh"]

CMD ["php-fpm"]
