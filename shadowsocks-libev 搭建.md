---
title: shadowsocks-libev 搭建
tags: gfw
GA: UA-131051587-2
---
[**主頁**](https://hackmd.io/@xrp4k0iHSfeGBDMiQ8kkzQ/SkaWsunMB/%2FuOfRBTx0SAq7xMx426pIUg)
![](https://i.imgur.com/reHGdZ1.png =50x)

# shadowsocks-libev 搭建
## Server
#### ubuntu中安裝shadowsocks-libev
```
apt-get install shadowsocks-libev
```

### server 啟動指令
在Ubuntu 終端中輸入：
```
ss-server \
-s 0.0.0.0 \
-p 遠端主機端口 \
-k 密碼 \
-m xchacha20-ietf-poly1305 \
-t 600 \
-d 8.8.8.8 \
--fast-open 
```
**建議的加密算法**
* chacha20-ietf-poly1305、xchacha20-ietf-poly1305
* **加速** aes-128-gcm、aes-192-gcm、aes-256-gcm

---

## Client
### mac os 中安裝shadowsocks-libev
```
brew install shadowsocks-libev
```
### 一般client啟動指令
在Mac os 終端輸入：
```
ss-local \
-s 遠端主機IP \
-p 遠端主機端口 \
-k 密碼 \
-l 本地socks5 端口 \
-m xchacha20-ietf-poly1305 \
-t 600
--fast-open
```

## 參考文獻
* [shadowsocks/shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)