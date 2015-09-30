# Image is based on https://registry.hub.docker.com/u/phusion/baseimage/
# and the php contains the Dockerfile.
FROM ubuntu:14.04


# PHP Packages.
RUN apt-get update

RUN apt-get install -y \
  php5-cli \
  php5-mysql \
  php5-curl \
  curl \
  mysql-client \
  wget \
  git \
  unzip

RUN apt-get clean

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

# Install Drush 7.
RUN composer global require drush/drush:7.*
RUN composer global update
# Unfortunately, adding the composer vendor dir to the PATH doesn't seem to work. So:
RUN ln -s /root/.composer/vendor/bin/drush /usr/local/bin/drush

# Setup PHP.
#RUN sed -i 's/display_errors = Off/display_errors = On/' /etc/php5/apache2/php.ini
#RUN sed -i 's/display_errors = Off/display_errors = On/' /etc/php5/cli/php.ini

# Add kraftwagen
RUN mkdir ~/.drush/
RUN git clone "git://github.com/kraftwagen/kraftwagen.git" ~/.drush/kraftwagen
RUN drush cc drush

RUN ls -la /home/

RUN mkdir /home/drupal

COPY ./src /home/drupal/src

RUN ls -la /home/drupal

RUN bash /home/drupal/src/build.sh

RUN mkdir /var/www

RUN ln -sF /home/drupal/build /var/www/html

RUN ls -la /var/www/html/*

VOLUME /var/www/html
