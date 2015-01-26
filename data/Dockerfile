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

RUN mkdir -p /root/scripts/

ADD config/nginx/   /etc/nginx/
ADD config/apache/  /etc/apache2/
ADD config/fpm/     /etc/php5/fpm/pool.d/
ADD scripts/        /root/scripts

## Set envs
ENV HOME /root
ENV USER root
WORKDIR /root/

CMD /bin/bash