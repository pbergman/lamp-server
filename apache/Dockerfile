FROM        debian:stable
MAINTAINER  Philip Bergman <philip@zicht.nl>

RUN echo "deb http://http.debian.net/debian stable main contrib non-free" > /etc/apt/sources.list
RUN apt-get update && apt-get upgrade --show-upgraded -y
RUN apt-get install apache2 \
                    libapache2-mod-fastcgi \
                    nano \
                    apache2-mpm-worker \
                    apache2-threaded-dev \
                    apache2-utils -y

RUN a2enmod alias \
            rewrite \
            setenvif \
            headers \
            fastcgi \
            actions \
            deflate \
            expires

RUN /etc/init.d/apache2 stop
RUN echo "ServerName localhost" >>  /etc/apache2/apache2.conf
ADD config/php-fpm.conf             /etc/apache2/conf.d/
ADD config/mpm-worker.conf          /etc/apache2/conf.d/
RUN mkdir -p /etc/php5/fpm/sockets/

VOLUME ["/etc/apache2/sites-available/", \
        "/etc/apache2/sites-enabled/", \
        "/var/www/", \
        "/etc/php5/fpm/sockets/"]

# Expose port
EXPOSE 80
# Run command
CMD apachectl -d /etc/apache2/ -f apache2.conf -e info -DFOREGROUND