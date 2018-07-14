FROM containerstack/alpine:3.7.0-armhf
LABEL maintainer="Remon Lam <remon@containerstack.io>"

RUN apk --update add mariadb mariadb-client pwgen && \
    rm -f /var/cache/apk/*

ADD entrypoint.sh /entrypoint.sh
#RUN mkdir /scripts/pre-exec.d && \
#    mkdir /scripts/pre-init.d && \
#    chmod -R 755 /scripts

EXPOSE 3306

VOLUME ["/var/lib/mysql"]

ENTRYPOINT ["/entrypoint.sh"]
