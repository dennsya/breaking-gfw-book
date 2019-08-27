---
title: shadowsocksR
tags: gfw
GA: UA-131051587-2
---
[**主頁**](https://hackmd.io/@xrp4k0iHSfeGBDMiQ8kkzQ/SkaWsunMB/%2FuOfRBTx0SAq7xMx426pIUg)
![](https://i.imgur.com/tGcKvBO.png =50x)

# shadowsocksR
[***shadowsocksr-backup/shadowsocksr***](https://github.com/shadowsocksr-backup/shadowsocksr/tree/manyuser/shadowsocks)
**參數建議文獻**
* [**ssr和ss链接解码(decode)**](https://www.kingr.top/2018/10/07/ssr-decode/)
* [ShadowsocksR 配置](https://www.zfl9.com/ssr.html)
* [***ShadowSocksR(SSR)功能详细介绍及使用教程***](https://www.quchao.net/ShadowsocksR.html?fbclid=IwAR22Zxg1u4Zkka82c6VxtmNuTlsAdINJYXnxDgItuyqM5tjZ6d0Yz-5KF14) 超級無敵詳細
* [[ShadowsocksR] 大概是萌新也看得懂的SSR功能详细介绍&使用教程](https://moe.best/tutorial/shadowsocksr.html?fbclid=IwAR0yVZmo6Xx3UWYdQcHCHgRh_M8YWjfX0p7DKcuAKkzVQ6gxUlXsHQJebU4)

## Server
環境
```
apt-get install python git
```

安裝
```
git clone https://github.com/shadowsocksrr/shadowsocksr.git ;\
cd shadowsocksr ;\
git checkout akkariiin/dev ;\
cd shadowsocks
```

啟動

```
python server.py \
-p 遠端主機端口 \
-k 密碼 \
-m aes-256-cfb \
-o plain \
-O origin 
```

**參數配置** 
* ``-o``： ``http_simple``, ~~``tls1.2_ticket_auth``~~，``-p``設置為 80、~~443~~。
    * 進階效果更好：[**SSR混淆及混淆协议参数的设置**](https://sobaigu.com/how-to-use-ssr-obfs.html)
* `-O` ： `chain_a`, `chain_b`, `auth_aes128_md5`或`auth_aes128_sha1`
    * 選`chain_a`, `chain_b`的話，`-m`就選`none`，因為本身自帶`RC4`

[***ShadowsocksR 协议插件文档***](https://github.com/shadowsocksr-backup/shadowsocks-rss/blob/master/ssr.md)


**參數選項:**

| 混淆插件 `-o`          | 協議定義插件 `-O`  |
|------------------------|--------------------|
| ~~plain~~              | ~~origin~~         |
| http_simple            | ~~verify_deflate~~ |
| http_post              | ~~verify_sha1~~    |
| ~~random_head~~        | ~~auth_sha1~~      |
| **tls1.2_ticket_auth** | ~~auth_sha1_v2~~   |
|                        | ~~auth_sha1_v4~~   |
|                        | auth_aes128_md5    |
|                        | auth_aes128_sha1   |
|                        | **auth_chain_a**   |
|                        | **auth_chain_b**   |

---

## Client
環境
```
brew install python git
```

安裝
```
git clone https://github.com/shadowsocksrr/shadowsocksr.git ;\
cd shadowsocksr ;\
git checkout akkariiin/dev ;\
cd shadowsocks
```
啟動
```
python local.py \
-s 遠端主機IP \
-p 遠端主機端口 \
-k 密碼 \
-m aes-256-cfb \
-O origin \
-o plain
```