FROM        debian:stable
MAINTAINER  Philip Bergman <philip@zicht.nl>

## Install packages
RUN apt-get update
RUN apt-get install -y mysql-server busybox -y
RUN /bin/busybox --install

## Set folders
RUN mkdir -p /var/run/mysqld/ /var/log/mysql/ /var/lib/mysql/
VOLUME ["/var/lib/mysql", "/root/"]

## Set envs
ENV HOME /root
ENV USER root

# Ad Config
ADD config/my.cnf /etc/mysql/my.cnf
# Add start script
ADD config/start /etc/mysql/start

# Expose port
EXPOSE 3306

# Run command
CMD /etc/mysql/start
