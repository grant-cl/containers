FROM alpine:3.22

RUN apk upgrade --no-cache \
    && apk add --no-cache \
       php php-fpm php-cli php-pear php-pdo_mysql php-json php-session php-gmp php-gd php-sockets php-gettext php-mbstring \
       php-ctype php-ldap php-curl php-snmp php-openssl php-simplexml php-pcntl php-iconv php-opcache php-posix php-dom \
       iputils fping \
       composer && \
       ln -sf /dev/stderr /var/log/php83/error.log && \
       ln -sf /dev/stderr /var/log/php83/www-error.log 

COPY www.conf /etc/php83/php-fpm.d/www.conf

ENTRYPOINT ["/usr/sbin/php-fpm83", "-F"]
