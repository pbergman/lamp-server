FROM        debian:wheezy
MAINTAINER  Philip Bergman <philip@zicht.nl>

# Add apt sources for wheezy
ADD config/apt.wheezy.tar.gz /

# Add dotdeb keys
RUN gpg --keyserver keys.gnupg.net --recv-key 89DF5277
RUN gpg -a --export 89DF5277 | apt-key add -

# Update and upgrade
RUN apt-get update
RUN apt-get upgrade -y

# Install packages
RUN apt-get install -y \
    php5-cli \
    php5-fpm \
    php5-gd \
    php5-xdebug \
    php5-curl \
    php5-apc \
    php5-mysql

# Setup php (config) folder
RUN cd /etc/php5/cli && ln -sf ../fpm/php.ini .
ADD config/php5/fpm /etc/php5/fpm

# Make log folder
RUN mkdir -p /var/log/fpm/
RUN mkdir -p /etc/php5/fpm/sockets/

VOLUME ["/var/www/", \
        "/etc/php5/fpm/pool.d/", \
        "/etc/php5/fpm/sockets/"]

# Run fpm in fore ground
CMD ["/usr/sbin/php5-fpm"]