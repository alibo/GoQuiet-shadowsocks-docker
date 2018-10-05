FROM imhang/shadowsocks-docker:latest

ARG GOQUIET_VER=1.2.1

ENV GQ_WEBSERVER_ADDRESS=1.1.1.1:443
ENV GQ_KEY=exampleconftest
ENV GQ_FAST_OPEN=false

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

ENTRYPOINT ss-server -p $SS_PORT -k $SS_PASSWORD -m $SS_METHOD -t $SS_TIMEOUT -d 1.1.1.1 -d 8.8.8.8 -u --plugin /usr/local/bin/gq-server --plugin-opts "/etc/goquiet/gqserver.json" 

