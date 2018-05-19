# GoQuiet-shadowsocks-docker
A Docker image for [Shadowsocks](https://github.com/shadowsocks) over [GoQuiet](https://github.com/cbeuw/GoQuiet/)


[![Docker Automated build](https://img.shields.io/docker/automated/alibo/goquiet-shadowsocks-docker.svg?style=flat-square)](https://hub.docker.com/r/alibo/goquiet-shadowsocks-docker/)


## What is GoQuiet?

>A shadowsocks plugin that obfuscates the traffic as normal HTTPS traffic and disguises the proxy server as a normal webserver.

>The fundamental idea of obfuscating shadowsocks traffic as TLS traffic is not original. [simple-obfs](https://github.com/shadowsocks/simple-obfs) and [ShadowsocksR](https://github.com/shadowsocksrr/shadowsocksr)'s `tls1.2_ticket_auth` mode have shown this to be effective. This plugin has made [improvements](https://github.com/cbeuw/GoQuiet/wiki/Advantages-over-similar-obfuscators) so that the goal of this plugin is to make indiscriminate blocking of HTTPS servers (or even IP ranges) with high traffic the only effective way of stopping people from using shadowsocks.

>Beyond the benefit of bypassing the firewall, it can also cheat traffic restrictions imposed by ISP. See [here](https://github.com/cbeuw/GoQuiet/wiki/A-potential-gateway-to-free-internet-after-Net-Neutrality-Repeal).

*Source: https://github.com/cbeuw/GoQuiet/*

## How to configure the server

GoQuiet works in two modes:

1. Running as a shadowsocks plugin
2. Standalone

### Shadowsocks Plugin

```bash
docker run -d --restart=always \
    -e "SS_PORT=443" \
    -e "SS_PASSWORD=shadowsocks_123456" \
    -e "SS_METHOD=chacha20-ietf-poly1305" \
    -e "SS_TIMEOUT=600" \
    -e "GQ_WEBSERVER_ADDRESS=1.1.1.1:443" \
    -e "GQ_KEY=exampleconftest" \
    -e "GQ_FAST_OPEN=true" \
    -p 443:443 -p 443:443/udp --name goquiet-node-1 alibo/goquiet-shadowsocks-docker:server
```


### Standalone

```bash
docker run -d --restart=always \
    -e "GQ_WEBSERVER_ADDRESS=1.1.1.1:443" \
    -e "GQ_KEY=exampleconftest" \
    -e "GQ_FAST_OPEN=true" \
    -e "GQ_SS_HOST=<shadowsocks-ip>" \
    -e "GQ_SS_PORT=8388" \
    -e "GQ_HOST=0.0.0.0" \
    -e "GQ_PORT=443" \
    -p 443:443 -p 443:443/udp --name goquiet-node-2 alibo/goquiet-shadowsocks-docker:server-standalone
```

You need a running instance of shadowsocks to redirect the traffic. You may want to use image [imhang/shadowsocks-docker](https://github.com/hangim/shadowsocks-docker):

```bash
docker run -d --restart=always \
    -e "SS_PORT=8388" \
    -e "SS_PASSWORD=shadowsocks_123456" \
    -e "SS_METHOD=chacha20-ietf-poly1305" \
    -e "SS_TIMEOUT=600" \
    -p 8388:8388 -p 8388:8388/udp --name shadowsocks-node-1 imhang/shadowsocks-docker
```

## Usage

### Docker

##### Shadowsocks + GoQuiet as a plugin

```bash
docker run -d --restart=always \
    -e "GQ_SERVER_NAME=www.bing.com" \
    -e "GQ_KEY=exampleconftest" \
    -e "GQ_FAST_OPEN=true" \
    -e "GQ_BROWSER=chrome" \
    -e "GQ_TICKET_TIME_HINT=3600" \
    -e "SS_SERVER_ADDRESS=<server-ip>" \
    -e "SS_SERVER_PORT=443" \
    -e "SS_LOCAL_ADDRESS=0.0.0.0" \
    -e "SS_LOCAL_PORT=1080" \
    -e "SS_PASSWORD=shadowsocks_123456" \
    -e "SS_METHOD=chacha20-ietf-poly1305" \
    -e "SS_TIMEOUT=600" \
    -p 1080:1080 --name goquiet-client-plugin-node-1 alibo/goquiet-shadowsocks-docker:client
```

##### Standalone

Waiting for the PR: https://github.com/cbeuw/GoQuiet/pull/34



### Windows

1. Download the client from [here](https://github.com/cbeuw/GoQuiet/releases)
2. See: https://github.com/cbeuw/GoQuiet/wiki/Instructions-for-Windows-Client-Users


### Mac, Linux

1. Download the client from [here](https://github.com/cbeuw/GoQuiet/releases)
2. See: https://github.com/cbeuw/GoQuiet#usage


### Android

1. Download GoQuiet plugin for [Shadowsocks Android Client](https://play.google.com/store/apps/details?id=com.github.shadowsocks):

https://github.com/cbeuw/GoQuiet-android/releases


## Credits

- [GoQuiet](https://github.com/cbeuw/GoQuiet/)
- [Shadowsocks](https://github.com/shadowsocks)
- [Shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)
- [hangim/shadowsocks-docker](https://github.com/hangim/shadowsocks-docker)

## License

MIT
