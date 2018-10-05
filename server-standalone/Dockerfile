FROM alpine:3.7

ARG GOQUIET_VER=1.2.1

ENV GQ_WEBSERVER_ADDRESS=1.1.1.1:443
ENV GQ_KEY=exampleconftest
ENV GQ_FAST_OPEN=false

ENV GQ_SS_HOST=127.0.0.1
ENV GQ_SS_PORT=8388
ENV GQ_HOST=0.0.0.0
ENV GQ_PORT=443

RUN \
    apk add --no-cache wget \
    && wget -O /usr/local/bin/gq-server https://github.com/cbeuw/GoQuiet/releases/download/v$GOQUIET_VER/gq-server-linux-386-$GOQUIET_VER \
    && chmod +x /usr/local/bin/gq-server

RUN \
    mkdir -p /etc/goquiet/ \
    && echo -e "{\n\
    \"WebServerAddr\":\"$GQ_WEBSERVER_ADDRESS\",\n\
    \"Key\":\"$GQ_KEY\",\n\
    \"FastOpen\":$GQ_FAST_OPEN\n\
    }"\
    > /etc/goquiet/gqserver.json

EXPOSE $GQ_PORT/tcp $GQ_PORT/udp

ENTRYPOINT /usr/local/bin/gq-server -s $GQ_HOST -p $GQ_PORT -r "$GQ_SS_HOST:$GQ_SS_PORT" -c /etc/goquiet/gqserver.json