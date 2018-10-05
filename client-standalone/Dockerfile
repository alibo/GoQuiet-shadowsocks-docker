FROM alpine:3.7

ARG GOQUIET_VER=1.2.1

ENV GQ_SERVER_NAME=www.bing.com
ENV GQ_KEY=exampleconftest
ENV GQ_FAST_OPEN=false
ENV GQ_BROWSER=chrome
ENV GQ_TICKET_TIME_HINT=3600
ENV GQ_LOCAL_PORT=1984
ENV GQ_SERVER_ADDRESS=127.0.0.1
ENV GQ_SERVER_PORT=443

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

ENTRYPOINT /usr/local/bin/gq-client -s $GQ_SERVER_ADDRESS -b "0.0.0.0" -p $GQ_SERVER_PORT -l $GQ_LOCAL_PORT -c /etc/goquiet/gqclient.json