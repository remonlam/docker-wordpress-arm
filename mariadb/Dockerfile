FROM containerstack/alpine:3.7.0-armhf
#FROM containerstack/alpine:3.8.0

LABEL maintainer="Remon Lam <remon@containerstack.io>"

RUN apk update && apk upgrade && \
    apk add mariadb mysql-client pwgen && \
    rm -rf /var/cache/apk/*

ADD entrypoint /entrypoint

EXPOSE 3306
ENTRYPOINT ["/entrypoint"]
