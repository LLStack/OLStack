FROM ubuntu:18.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install apt-utils -y && \
    apt-get install wget curl tzdata dialog -y

RUN apt-get install -y software-properties-common && \
    add-apt-repository universe && \
    add-apt-repository ppa:certbot/certbot && \
    apt-get update && \
    apt-get install -y certbot

ENV olsver 1.6.8
ENV phpver 73

RUN curl -O https://openlitespeed.org/preuse/openlitespeed-$olsver.tgz && \
    tar xvf openlitespeed-$olsver.tgz && cd openlitespeed && \
    LINENUM=$(grep -n "LSPHPVER=" _in.sh | cut -d: -f 1) && \
    sed -i "${LINENUM}d" _in.sh && \
    sed -i "${LINENUM}iLSPHPVER=${phpver}" _in.sh && \
    ./install.sh && \
	rm -rf ../openlitespeed ../openlitespeed-$olsver.tgz

RUN apt-get install lsphp$phpver lsphp$phpver-common lsphp$phpver-mysql lsphp$phpver-opcache \
    lsphp$phpver-curl lsphp$phpver-json lsphp$phpver-imagick lsphp$phpver-redis lsphp$phpver-memcached \
    lsphp$phpver-sqlite3 lsphp$phpver-imagick lsphp$phpver-ldap lsphp$phpver-recode -y

RUN wget -O /usr/local/lsws/admin/misc/lsup.sh \
    https://raw.githubusercontent.com/litespeedtech/openlitespeed/master/dist/admin/misc/lsup.sh && \
    chmod +x /usr/local/lsws/admin/misc/lsup.sh && \
    ln -s /usr/local/lsws/lsphp$olsver/bin/php /usr/bin/php

EXPOSE 80 443 7080

RUN rm -rf /usr/local/lsws/conf
ADD conf /usr/local/lsws/conf
RUN chown 999:999 /usr/local/lsws/conf -R
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
WORKDIR /var/www/vhosts/
CMD ["/usr/local/lsws/bin/lswsctrl","start"]