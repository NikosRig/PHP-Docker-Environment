From ubuntu:latest

ARG UID
ARG GID

RUN apt-get update && apt-get install -y --no-install-recommends apache2

#Enable apache required modules
RUN a2enmod rewrite proxy proxy_fcgi proxy_http \
#Add server name in configuration
  && echo 'ServerName mailman' >> /etc/apache2/apache2.conf \
#Create group
  && groupadd develgroup -g $GID \
#Create user
  && useradd -ms /bin/bash develuser -u $UID -g $GID \
  && chown -R develuser:develgroup /home/develuser && chmod -R 775 /home/develuser \
#Change apache's default user/group to ours
  && sed -i -e"s/^export\s*APACHE_RUN_USER=www-data/export APACHE_RUN_USER=develuser/" /etc/apache2/envvars \
  && sed -i -e"s/^export\s*APACHE_RUN_GROUP=www-data/export APACHE_RUN_GROUP=develgroup/" /etc/apache2/envvars

#Copy new virtual host in container
COPY VirtualHost.conf /etc/apache2/sites-available/

#Disable default virtual host and enable ours
RUN a2dissite 000-default.conf && a2ensite VirtualHost.conf \
&& chown -R develuser:develgroup /home/develuser && chmod -R 775 /home/develuser

#Run apache
CMD apache2ctl -D FOREGROUND
