FROM        pbergman/busybox:latest
MAINTAINER  Philip Bergman <philip@zicht.nl>

VOLUME ["/etc/apache2/sites-available/", \
        "/etc/apache2/sites-enabled/", \
        "/etc/nginx/sites-available/", \
        "/etc/nginx/sites-enabled/", \
        "/var/www/", \
        "/etc/php5/fpm/pool.d/", \
        "/etc/php5/fpm/sockets/", \
        "/var/lib/mysql", \
        "/root/"]

ADD config/nginx/   /etc/nginx/
ADD config/apache/  /etc/apache2/
ADD config/fpm/     /etc/php5/fpm/pool.d/

CMD /bin/bash