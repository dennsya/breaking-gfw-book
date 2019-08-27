---
title: shadowsocks-libev + simple-obfs 搭建
tags: gfw
GA: UA-131051587-2
---

![](https://i.imgur.com/reHGdZ1.png =50x)

# shadowsocks-libev + simple-obfs 搭建

## Server
> 請在**root**執行
> **環境Ubuntu**

### 安裝環境
安裝shadowsocks-libev
```
apt-get install shadowsocks-libev -y
```
安裝simple-obfs:
```
apt-get install --no-install-recommends build-essential autoconf libtool libssl-dev libpcre3-dev libev-dev asciidoc xmlto automake -y
git clone https://github.com/shadowsocks/simple-obfs.git \
cd simple-obfs \
git submodule update --init --recursive \
./autogen.sh \
./configure && make \
make install
```

### 啟動 Server(http/tls)

```
ss-server \
-s 0.0.0.0 \
-p 遠端主機端口 \
-k 密碼 \
-m xchacha20-ietf-poly1305 \
-t 600 \
-d 8.8.8.8 \
--fast-open \
--plugin obfs-server \
--plugin-opts "obfs=http"
```
* ```obfs=http```把```http``` 改成 ```tls```模式就切換啦

**建議的加密算法**
* chacha20-ietf-poly1305、xchacha20-ietf-poly1305
* **加速** aes-128-gcm、aes-192-gcm、aes-256-gcm

---

## Client
> **環境mac os**

### 安裝環境
安裝shadowsocks-libev
```
brew upgrade shadowsocks-libev
```
安裝simple-obfs:
```
brew install simple-obfs
```

### 啟動 Client (http/tls)

```
ss-local \
-s 遠端主機IP \
-p 遠端轉機端口 \
-k 密碼 \
-l 本地socks5端口 \
-m xchacha20-ietf-poly1305 \
-t 600 \
--fast-open \
--plugin obfs-local \
--plugin-opts "obfs=http;obfs-host=www.bing.com"
```
* ```host=www.bing.com```挑一個中國響叮噹的域名
* ```obfs=http```把```http``` 改成 ```tls```模式就切換啦

## simple-obfs 參考文獻：
* [shadowsocks/simple-obfs](https://github.com/shadowsocks/simple-obfs)
* [shadowsocks/simple-obfsclient 指令](https://github.com/shadowsocks/simple-obfs/blob/master/doc/obfs-local.asciidoc)
* [shadowsocks/simple-obfsserver 指令](https://github.com/shadowsocks/simple-obfs/blob/master/doc/obfs-server.asciidoc)
* [MacOS使用shadowsocks-libev+Simple-OBFS教程](https://medium.com/@yanlong/macos%E4%BD%BF%E7%94%A8shadowsocks-libev-simple-obfs%E6%95%99%E7%A8%8B-c10eba9c0758)