FROM gcr.io/google-containers/debian-base-amd64:0.3

ARG VER
ENV HOME /bitcoin
EXPOSE 8332 8333
VOLUME /bitcoin/data
COPY entrypoint.sh /

RUN \
 echo "deb http://ftp.us.debian.org/debian unstable main" >> /etc/apt/sources.list && \
 echo "Package: *\nPin: release a=unstable\nPin-Priority: 800" >> /etc/apt/preferences && \
 apt-get update && apt-get install -y --no-install-recommends bash gnupg dirmngr ca-certificates wget bitcoind && \
 # gosu
 gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 && \
 wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/1.10/gosu-$(dpkg --print-architecture)" && \
 wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/1.10/gosu-$(dpkg --print-architecture).asc" && \
 gpg --verify /usr/local/bin/gosu.asc && \
 rm /usr/local/bin/gosu.asc && \
 chmod +x /usr/local/bin/gosu && \
 # files
 chmod 0755 /entrypoint.sh && \
 useradd -U -M -s /usr/sbin/nologin -d /bitcoin bitcoin && \
 mkdir -p /bitcoin/data && \
 mkdir -p /bitcoin/.bitcoin && \
 ln -s /etc/bitcoin.conf /bitcoin/.bitcoin/bitcoin.conf && \
 chown -R bitcoin: /bitcoin && \
 # clean
 apt-get purge -y --auto-remove ca-certificates wget gnupg dirmngr && \
 apt-get clean && rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["/entrypoint.sh"]
