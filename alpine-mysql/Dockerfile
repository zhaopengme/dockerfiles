FROM alpine:latest
MAINTAINER zhaopengme "user.zhaopeng@gmail.com"

RUN apk update && \
	apk add mysql mysql-client && \
	addgroup mysql mysql && \
	mkdir /scripts && \
	rm -rf /var/cache/apk/*

VOLUME ["/var/lib/mysql"]

COPY ./startup.sh /scripts/startup.sh
RUN chmod +x /scripts/startup.sh

COPY ./my.cnf /etc/mysql/my.cnf
COPY ./server.cnf /etc/mysql/server.cnf
COPY ./client.cnf /etc/mysql/client.cnf

EXPOSE 3306

ENTRYPOINT ["/scripts/startup.sh"]
