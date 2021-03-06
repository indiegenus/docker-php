FROM kooldev/php:7.2

ENV PHP_FPM_LISTEN=/run/php-fpm.sock \
    NGINX_LISTEN=80 \
    NGINX_ROOT=/app/public \
    NGINX_INDEX=index.php \
    NGINX_CLIENT_MAX_BODY_SIZE=25M \
    NGINX_PHP_FPM=unix:/run/php-fpm.sock \
    NGINX_FASTCGI_READ_TIMEOUT=60s \
    NGINX_FASTCGI_BUFFERS='8 8k' \
    NGINX_FASTCGI_BUFFER_SIZE='16k'

RUN curl -L https://github.com/ochinchina/supervisord/releases/download/v0.6.3/supervisord_static_0.6.3_linux_amd64 -o /usr/local/bin/supervisord \
    && chmod +x /usr/local/bin/supervisord \
    && apk add --no-cache nginx \
    && sed -i "s|^user .*|user\ kool kool;|g" /etc/nginx/nginx.conf \
    && chown -R kool:kool /var/lib/nginx \
    && chmod 770 /var/lib/nginx/tmp \
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log

COPY supervisor.conf /kool/supervisor.conf
COPY default.tmpl /kool/default.tmpl
COPY entrypoint /kool/entrypoint
RUN chmod +x /kool/entrypoint

EXPOSE 80

CMD [ "supervisord", "-c", "/kool/supervisor.conf" ]
