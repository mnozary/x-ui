# x-ui

Multi-protocol multi-user xray panel support

# Introduction

- Monitoring status system
- Support multi-user multi-protocol, website operation visualization
- Supported protocols: vmess、vless、trojan、shadowsocks、dokodemo-door、socks、http
- Support configuration more configuration transfer
- Statistics flow, limit flow, limit expiration time
- Customizable xray template configuration
- Support https access panel
- Support one-time SSL certificate application 且automatic renewal
- More advanced configuration, detailed panel

# Installation & upgrade

```
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```

## Manual installation & upgrade

1. First, download the latest compressed package from https://github.com/vaxilu/x-ui/releases, and generally choose the `amd64` architecture
2. Then upload this compressed package to the `/root/` directory of the server, and log in to the server as the `root` user

> If your server cpu architecture is not `amd64`, replace `amd64` in the command with other architectures

```
cd /root/
rm x-ui/ /usr/local/x-ui/ /usr/bin/x-ui -rf
tar zxvf x-ui-linux-amd64.tar.gz
chmod +x x-ui/x-ui x-ui/bin/xray-linux-* x-ui/x-ui.sh
cp x-ui/x-ui.sh /usr/bin/x-ui
cp -f x-ui/x-ui.service /etc/systemd/system/
mv x-ui/ /usr/local/
systemctl daemon-reload
systemctl enable x-ui
systemctl restart x-ui
```

## Install using docker

> This docker tutorial and docker image are provided by [Chasing66](https://github.com/Chasing66)

1. Install Docker

```shell
curl -fsSL https://get.docker.com | sh
```

2. Install x-ui

```shell
mkdir x-ui && cd x-ui
docker run -itd --network=host \
    -v $PWD/db/:/etc/x-ui/ \
    -v $PWD/cert/:/root/cert/ \
    --name x-ui --restart=unless-stopped \
    enwaiax/x-ui:latest
```

> Build your own image

```shell
docker build -t x-ui .
```

## SSL Certificate Application

> This feature and tutorial are provided by [FranzKafkaYu](https://github.com/FranzKafkaYu)

The script has a built-in SSL certificate application function. To use this script to apply for a certificate, the following conditions must be met:

- Know the Cloudflare registration email address
- Know the Cloudflare Global API Key
- The domain name has been resolved to the current server through Cloudflare

How to obtain the Cloudflare Global API Key:
    ![](media/bda84fbc2ede834deaba1c173a932223.png)
    ![](media/d13ffd6a73f938d1037d0708e31433bf.png)

When using, you only need to enter `domain name`, `email`, `API KEY`, as shown in the diagram below:
![](media/2022-04-04_141259.png)

Notes:

- This script uses DNS API for certificate application
- Let'sEncrypt is used as CA by default
- The certificate installation directory is /root/cert directory
- All certificates applied for by this script are wildcard domain name certificates

## Used by Tg robot (under development, temporarily unavailable)

> This function and tutorial are provided by [FranzKafkaYu](https://github.com/FranzKafkaYu)

X-UI supports daily traffic notification, panel login reminder and other functions through Tg robot. To use Tg robot, you need to apply for it yourself
For specific application tutorials, please refer to [blog link](https://coderfan.net/how-to-use-telegram-bot-to-alarm-you-when-someone-login-into-your-vps.html)
Instructions: Set robot-related parameters in the panel background, including

- Tg robot Token
- Tg robot ChatId
- Tg robot cycle running time, using crontab syntax

Reference syntax:
- 30 * * * * * //Notify at the 30th second of every minute
- @hourly //Notify every hour
- @daily //Notify every day (at midnight)
- @every 8h //Notify every 8 hours

TG notification content:
- Node traffic usage
- Panel login reminder
- Node expiration reminder
- Traffic warning reminder

More functions are being planned...
## Recommended system

- CentOS 7+
- Ubuntu 16+
- Debian 8+

# FAQ

## Migrate from v2-ui

First, install the latest version of x-ui on the server where v2-ui is installed, and then use the following command to migrate, which will migrate `all inbound account data` of the local v2-ui to x-ui, `panel settings and username and password will not be migrated`

> After the migration is successful, please `close v2-ui` and `restart x-ui`, otherwise the inbound of v2-ui will be different from the inbound of x-ui `Port conflict`

```
x-ui v2-ui
```

## issue closed

Various novice questions make my blood pressure go up

## Stargazers over time

[![Stargazers over time](https://starchart.cc/vaxilu/x-ui.svg)](https://starchart.cc/vaxilu/x-ui)
