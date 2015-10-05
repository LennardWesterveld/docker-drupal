# Image is based on https://registry.hub.docker.com/u/phusion/baseimage/
# and the php contains the Dockerfile.
FROM phusion/baseimage:0.9.16

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# Inspired by Tutum Apache PHP.
#
# https://registry.hub.docker.com/u/tutum/apache-php/dockerfile/
#
ENV DEBIAN_FRONTEND noninteractive

# Set timezone and locale.
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# PHP Packages.
RUN apt-get update && apt-get -y install libapache2-mod-php5 apache2 \
    php5 php5-fpm php5-gd php5-ldap \
    php5-sqlite php5-pgsql php-pear php5-mysql php5-curl \
    php5-mcrypt php5-xmlrpc php5-intl php5-xdebug \
    build-essential curl

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

EXPOSE 80

# Prepare apache
RUN a2enmod rewrite alias

# Exporting current path to make tools available when using nsenter.
RUN echo "export PATH=$PATH" >> $HOME/.bashrc \
    && echo "export TERM=xterm" >> $HOME/.bashrc

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir /etc/service/apache
ADD services/apache.sh /etc/service/apache/run