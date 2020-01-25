FROM ubuntu:14.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install apt-utils -y && \
    apt-get install -y wget curl tzdata dialog  && \
    apt-get install -qq -y --no-install-recommends python python-dev gcc libssl-dev openssl libffi-dev ca-certificates libaugeas0 augeas-lenses python-virtualenv

RUN wget https://dl.eff.org/certbot-auto && \
    mv certbot-auto /usr/bin/certbot && \
    chown root /usr/bin/certbot && \
    chmod 0755 /usr/bin/certbot && \
    /bin/sh /usr/bin/certbot --install-only 

ENV olsver 1.6.6
ENV phpver 53

RUN curl -O https://openlitespeed.org/preuse/openlitespeed-$olsver.tgz && \
    tar xvf openlitespeed-$olsver.tgz && cd openlitespeed && \
    LINENUM=$(grep -n "LSPHPVER=" _in.sh | cut -d: -f 1) && \
    sed -i "${LINENUM}d" _in.sh && \
    sed -i "${LINENUM}iLSPHPVER=${phpver}" _in.sh && \
    ./install.sh && \
	rm -rf ../openlitespeed ../openlitespeed-$olsver.tgz

RUN apt-get install lsphp$phpver lsphp$phpver-* -y && \
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