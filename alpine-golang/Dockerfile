FROM alpine:latest
MAINTAINER mikey.zhaopeng "user.zhaopeng@gmail.com"

RUN apk update && apk add ca-certificates
WORKDIR /app

ADD start.sh /app/start.sh
RUN chmod +x /app/start.sh
RUN mkdir /lib64 && ln -s /lib/libc.musl-x86_64.so.1 /lib64/ld-linux-x86-64.so.2

EXPOSE 80

ENTRYPOINT ["/app/start.sh"]
