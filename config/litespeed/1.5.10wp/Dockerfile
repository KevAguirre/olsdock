#FROM debian:jessie-slim
FROM ubuntu:18.04

RUN apt-get update && apt-get install -y wget && \
    wget https://openlitespeed.org/preuse/openlitespeed-1.5.10.tgz && \
    tar xzvf openlitespeed-1.5.10.tgz && cd openlitespeed && \
    ./install.sh && \
    rm -rf /openlitespeed && \
    apt-get install -y curl mysql-client \
    lsphp73 lsphp73-mysql lsphp73-opcache lsphp73-curl

RUN echo "admin:$(/usr/local/lsws/admin/fcgi-bin/admin_php* -q /usr/local/lsws/admin/misc/htpasswd.php ${WEB_ADMIN})" \
    > /usr/local/lsws/admin/conf/htpasswd

EXPOSE 7080

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
	chmod +x wp-cli.phar && mv wp-cli.phar /usr/local/bin/wp && \
	ln -s /usr/local/lsws/lsphp73/bin/php7.3 /usr/bin/php

#install certbot
#RUN apt-get install -y software-properties-common && \
#    add-apt-repository 'deb http://ftp.debian.org/debian jessie-backports main' && \
#    apt-get update && apt-get install -y python-certbot-apache -t jessie-backports

# Download postfix to send mails
#RUN apt-get update && \
#  DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y \
#    postfix \
#    bsd-mailx \
#    mysql-client && \
#    mkfifo /var/spool/postfix/public/pickup

#remove the conf files and add the template conf files instead
RUN rm -rf /usr/local/lsws/conf
ADD conf /usr/local/lsws/conf
RUN chown 999 /usr/local/lsws/conf -R
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
WORKDIR /var/www/vhosts/

CMD ["/usr/local/lsws/bin/openlitespeed","-n"]

