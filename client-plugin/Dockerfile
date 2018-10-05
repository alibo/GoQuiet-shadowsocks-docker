FROM imhang/shadowsocks-docker:latest

ARG GOQUIET_VER=1.2.1

ENV GQ_SERVER_NAME=www.bing.com
ENV GQ_KEY=exampleconftest
ENV GQ_FAST_OPEN=false
ENV GQ_BROWSER=chrome
ENV GQ_TICKET_TIME_HINT=3600

ENV SS_SERVER_ADDRESS=127.0.0.1
ENV SS_SERVER_PORT=443
ENV SS_LOCAL_ADDRESS=0.0.0.0
ENV SS_LOCAL_PORT=1080

RUN \
    apk add --no-cache wget \
    && wget -O /usr/local/bin/gq-client https://github.com/cbeuw/GoQuiet/releases/download/v$GOQUIET_VER/gq-client-linux-386-$GOQUIET_VER \
    && chmod +x /usr/local/bin/gq-client

RUN \
    mkdir -p /etc/goquiet/ \
    && echo -e "{\n\
    \"ServerName\":\"$GQ_SERVER_NAME\",\n\
    \"TicketTimeHint\":$GQ_TICKET_TIME_HINT,\n\
    \"Browser\":\"$GQ_BROWSER\",\n\
    \"Key\":\"$GQ_KEY\",\n\
    \"FastOpen\":$GQ_FAST_OPEN\n\
    }"\
    > /etc/goquiet/gqclient.json

EXPOSE $SS_LOCAL_PORT

ENTRYPOINT ss-local -s $SS_SERVER_ADDRESS -p $SS_SERVER_PORT -b $SS_LOCAL_ADDRESS -l $SS_LOCAL_PORT -k $SS_PASSWORD -m $SS_METHOD -t $SS_TIMEOUT -u --plugin /usr/local/bin/gq-client --plugin-opts "/etc/goquiet/gqclient.json" 
