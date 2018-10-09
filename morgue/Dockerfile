FROM php:7.2-alpine

RUN apk add --no-cache apache2 php7-apache2 php7-session php7-curl php7-mysqli php7-openssl php7-json php7-phar php7-pdo php7-pdo_mysql postfix git

RUN cd /var/www/ \
  && git clone https://github.com/etsy/morgue.git morgue \
  && rm -f /var/www/morgue/config/example.json \
  && mkdir -p /run/apache2 \
  && chgrp www-data /run/apache2 \
  && chmod 775 /run/apache2 \
  && chown apache:apache logs \
  && chmod g+w /var/log/apache2 \
  && addgroup apache wheel \
  && cd /var/www/morgue \
  && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php composer-setup.php --install-dir=/var/www/morgue \
  && php -r "unlink('composer-setup.php');" \
  && php composer.phar update \
  && apk del git \
  && rm -rf /var/cache/apk/* \
  && rm -rf /var/www/localhost \
  && postconf "smtputf8_enable = no" \
  && postfix start

ADD httpd.conf /etc/apache2/httpd.conf

EXPOSE 80

ENTRYPOINT ["httpd","-D","FOREGROUND"]