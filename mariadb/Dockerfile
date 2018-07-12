FROM containerstack/alpine:3.8.0-armhf
#FROM containerstack/alpine:3.8.0

LABEL maintainer="Remon Lam <remon@containerstack.io>"

RUN apk update && apk upgrade && \
    apk add mariadb mariadb-client && \
    rm -rf /var/cache/apk/*

ADD entrypoint.sh /entrypoint.sh

EXPOSE 3306
ENTRYPOINT ["/entrypoint.sh"]
