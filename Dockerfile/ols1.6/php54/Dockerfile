FROM ubuntu:16.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install apt-utils -y && \
    apt-get install wget curl tzdata dialog -y && \
    apt-get install -qq -y libcurl3 libenchant1c2a libxpm4 libc-client2007e libmcrypt4 libaspell15 librecode0 \
                           libvpx-dev libtidy-0.99-0 libxslt1.1 libmysqlclient20 libpng12-0 libjpeg-turbo8 libfreetype6

RUN apt-get install -y software-properties-common && \
    add-apt-repository universe && \
    add-apt-repository ppa:certbot/certbot && \
    apt-get update && \
    apt-get install -y certbot

ENV olsver 1.6.6
ENV phpver 54

RUN curl -O https://openlitespeed.org/preuse/openlitespeed-$olsver.tgz && \
    tar xvf openlitespeed-$olsver.tgz && cd openlitespeed && \
    LINENUM=$(grep -n "LSPHPVER=" _in.sh | cut -d: -f 1) && \
    sed -i "${LINENUM}d" _in.sh && \
    sed -i "${LINENUM}iLSPHPVER=${phpver}" _in.sh && \
    ./install.sh && \
	rm -rf ../openlitespeed ../openlitespeed-$olsver.tgz

RUN apt-get install lsphp$phpver lsphp$phpver-curl lsphp$phpver-dbg lsphp$phpver-enchant lsphp$phpver-gd lsphp$phpver-gmp lsphp$phpver-imap \
                    lsphp$phpver-intl lsphp$phpver-ldap lsphp$phpver-mcrypt lsphp$phpver-mysql lsphp$phpver-pspell lsphp$phpver-recode lsphp$phpver-sqlite \
                    lsphp$phpver-tidy lsphp$phpver-xmlrpc lsphp$phpver-xsl && \
    mv /usr/local/lsws/fcgi-bin/lsphp /usr/local/lsws/fcgi-bin/lsphp-bak && \
    ln -sf /usr/local/lsws/lsphp$phpver/bin/lsphp /usr/local/lsws/fcgi-bin/lsphp && \
    ln -sf /usr/local/lsws/lsphp$phpver/bin/lsphp /usr/bin/lsphp

RUN wget -O /usr/local/lsws/admin/misc/lsup.sh \
    https://raw.githubusercontent.com/litespeedtech/openlitespeed/master/dist/admin/misc/lsup.sh && \
    chmod +x /usr/local/lsws/admin/misc/lsup.sh

EXPOSE 80 443 7080

RUN rm -rf /usr/local/lsws/conf
ADD conf /usr/local/lsws/conf
RUN chown 999:999 /usr/local/lsws/conf -R
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
WORKDIR /var/www/vhosts/
CMD ["/usr/local/lsws/bin/lswsctrl","start"]