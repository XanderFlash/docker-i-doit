#
# Builds a docker image for i-doit 1.13
#
FROM debian:buster
ENV idoit_home /var/www/html/i-doit
ENV idoit_download_url https://sourceforge.net/projects/i-doit/files/i-doit/1.13/idoit-open-1.13.zip/download 

COPY policy-rc.d /usr/sbin/policy-rc.d

RUN apt-get -qq --yes update
RUN apt-get -qq --yes full-upgrade
RUN apt-get -qq --yes clean
RUN apt-get -qq --yes autoremove

RUN apt-get -qq --yes update
RUN DEBIAN_FRONTEND=noninteractive apt-get -qq --yes install \
    apache2 libapache2-mod-php7.3 \
    mariadb-client mariadb-server \
    php7.3-bcmath php7.3-cli php7.3-common php7.3-curl php7.3-gd php7.3-imagick php7.3-json \
    php7.3-ldap php7.3-mbstring php7.3-memcached php7.3-mysql php7.3-pgsql \
    php7.3-soap php7.3-xml php7.3-zip \
    memcached unzip sudo moreutils wget

RUN mkdir -p ${idoit_home}

COPY i-doit.ini /etc/php/7.3/mods-available/i-doit.ini
RUN phpenmod i-doit
RUN phpenmod memcached

RUN a2dissite 000-default
COPY i-doit.conf /etc/apache2/sites-available/i-doit.conf
RUN a2ensite i-doit
RUN a2enmod rewrite
RUN a2enmod alias
RUN a2enmod proxy
RUN a2enmod setenvif

COPY 99-i-doit.cnf /etc/mysql/mariadb.conf.d/99-i-doit.cnf

RUN wget -O i-doit.zip ${idoit_download_url} \
	&&  unzip i-doit.zip -d ${idoit_home} \
	&& rm i-doit.zip

RUN chmod +x ${idoit_home}/idoit-rights.sh
RUN cd ${idoit_home} && ./idoit-rights.sh set

ENTRYPOINT service apache2 start && service mysql start && /bin/bash

EXPOSE 80
