FROM python:alpine3.7
MAINTAINER FAN VINGA<fanalcest@gmail.com>

ENV DOMAIN=your_domain \
    USER=sspanel       \
    PASSWD=sspanel     \
    DB=sspanel         \
    NODEID=3

RUN  apk --no-cache add --virtual .build-deps \
                        git \
                        tar \          
                        autoconf \
                        automake \
                        make \
                        build-base \
                        linux-headers && \
     apk --no-cache add --virtual runtime-tools \
                        curl \
                        libsodium-dev \
                        openssl-dev \
                        udns-dev \
                        mbedtls-dev \
                        pcre-dev \
                        libev-dev \
                        libtool \
                        libffi-dev && \
     git clone -b manyuser https://github.com/glzjin/shadowsocks.git "/root/shadowsocks" --depth 1 && \
     cd  /root/shadowsocks                    && \
     pip install -r requirements.txt          && \
     cp  apiconfig.py userapiconfig.py        && \
     cp  config.json  user-config.json        && \
     sed -i "s#API_INTERFACE = 'modwebapi'#API_INTERFACE = 'glzjinmod'#" /root/shadowsocks/userapiconfig.py                  && \
     apk del .build-deps 

WORKDIR /root/shadowsocks

CMD sed -i "s#MYSQL_HOST = '127.0.0.1'#MYSQL_HOST = '${DOMAIN}'#" /root/shadowsocks/userapiconfig.py && \
    sed -i "s#MYSQL_USER = 'ss'#MYSQL_USER = '${USER}'#" /root/shadowsocks/userapiconfig.py && \
    sed -i "s#MYSQL_PASS = 'ss'#MYSQL_PASS = '${PASSWD}'#" /root/shadowsocks/userapiconfig.py && \
    sed -i "s#MYSQL_DB = 'shadowsocks'#MYSQL_DB = '${DB}'#" /root/shadowsocks/userapiconfig.py && \
    sed -i "s#NODE_ID\ =\ 1#NODE_ID\ =\ ${NODEID}#" /root/shadowsocks/userapiconfig.py && \
    python /root/shadowsocks/server.py
