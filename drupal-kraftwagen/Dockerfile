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

RUN mkdir /var/www
RUN mkdir /var/www/drupal

COPY ./src /var/www/drupal/src

RUN ls -la /var/www/drupal/src/cnf

RUN cd /var/www/drupal && bash /var/www/drupal/src/build.sh

RUN ln -s /var/www/drupal/build /var/www/html

RUN ls -la /var/www/html/*

# Fix permissions
RUN groupadd -f -r www-data
RUN id -u www-data || useradd -r -g www-data www-data

RUN usermod -u 1000 www-data
RUN usermod -G staff www-data

RUN bash /var/www/drupal/src/cnf/fix-permissions.sh --drupal_path=/var/www/drupal/build --drupal_user=www-data --httpd_group=www-data

VOLUME /var/www/html
