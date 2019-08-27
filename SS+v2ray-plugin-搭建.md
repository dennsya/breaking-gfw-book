---
title: SS + v2ray-plugin 搭建
tags: gfw
GA: UA-131051587-2
---
[**主頁**](https://hackmd.io/@xrp4k0iHSfeGBDMiQ8kkzQ/SkaWsunMB/%2FuOfRBTx0SAq7xMx426pIUg)
![](https://i.imgur.com/reHGdZ1.png =50x)![](https://i.imgur.com/gsJ3ke3.png =50x)


# SS + v2ray-plugin 搭建

## 事前準備
### 1. 域名申請：https://my.freenom.com/
* 只要有心就可以找到免費的域名

> * 註冊完的樣子
> ![](https://i.imgur.com/jKn8ocQ.png)
> * 點擊Manage domain 將自己的想要的ＩＰ填寫進去
> ps. 可以ping 測試一下



### 2. 設定 cloudflare：https://www.cloudflare.com/

* cloudflare DNS-> 增加一條 A 记录；
    * name=域名
    * value=vpsIP
    * ttl=automatic
    * status=onlyDns
* cloudflare Crypto -> 
    * SSL = Flexible
    * 总是开启 HTTPS
* **很重要**：點擊overview分頁，會提示你回到 "https://my.freenom.com/" 修改一些 域名

---

## Server
### 1. go 環境搭建：
```
apt-get update;\
apt-get upgrade -y;\
wget https://dl.google.com/go/go1.12.6.linux-amd64.tar.gz;\
sudo tar -xvf go1.12.6.linux-amd64.tar.gz;\
sudo mv go /usr/local;\
export GOROOT=/usr/local/go;\
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```

### 2. build v2ray-plug(適用Linux / Mac):
```
git clone https://github.com/shadowsocks/v2ray-plugin.git;\
cd v2ray-plugin;\
go build;\
cp v2ray-plugin /usr/bin/v2ray-plugin
```
> ps. 如果希望可以一次噴出linux,mac, windows版本，在v2ray-plugin底下執行：
> 
> ```
> bash build-release.sh
> ```


### 3. shadowsocks-libev 安裝
```
apt-get install shadowsocks-libev
```

### 4. acme腳本自動申請證書申請

#### 4. 1 申請證書前
**準備：**

到cloudflare頁面，點擊頭像(右上角)，點擊我的個人資料，在個人信息頁面尋找 API KEYs - > 全球API KEY，點擊"view"，拷貝key。

**保存：**
* API KEY
* CloundFlare的登陸郵箱。

#### 4.2 申請證書

填入 4.1 保存的訊息，在server 終端執行
```
export CF_Email="原本註冊CloundFlare的mail"; \
export CF_Key="你的 API KEY"; \
curl https://get.acme.sh | sh; \
bash acme.sh/acme.sh --issue --dns dns_cf -d 你的域名
```

> ps. 這步驟常常遇到問題，請保重。
> **完成後可以在 ```/root/.acme.sh/你的域名/``` 看到一大堆生成的憑證鑰匙，要都有喔！**
> * 你的域名.cer
> * 你的域名.conf
> * 你的域名.csr
> * 你的域名.csr.conf
> * 你的域名.ga.key
> * ca.cer
> * fullchain.cer

### 5. 啟動ss-server
> 設置基本上與shadowsocks-libev沒有差別
> 注意事項：
> * 因為要模擬https，所以防火牆必須開放443端口（tcp/udp）都開
> ps 基本上所有的參數設定都可以在這裡找到(https://github.com/shadowsocks/shadowsocks-libev)
> 

```
ss-server \
-s 0.0.0.0 \
-k 密碼 \
-p 443 \
-d 8.8.8.8 \
-m aes-128-gcm \
--fast-open \
--reuse-port \
--no-delay \
--plugin v2ray-plugin \
--plugin-opts "\
server;\
tls;\
fast-open;\
host=自己申請的域名;\
loglevel=none"
```

* ```-p 443```模擬https，所以必須是。
* ```-plugin /usr/bin/v2ray-plugin```位置可以是絕對位置，如果止填寫```v2ray-plugin```默認位置為```/usr/bin/```
* **更換成 QUIC server** 修改```--plugin-opts "tls"```當中的```tls```改為```quic```就行。

**建議的加密算法**
* chacha20-ietf-poly1305、xchacha20-ietf-poly1305
* **加速** aes-128-gcm、aes-192-gcm、aes-256-gcm

---

## Client
```
ss-local \
-s 遠端主機IP\
-k 密碼 \
-p 443 \
-l 1080 \
-m aes-128-gcm \
-t 600 \
--fast-open \
--reuse-port \
--no-delay \
--plugin v2ray-plugin \
--plugin-opts "\
tls;\
fast-open;\
host=自己申請的域名"
```

* **更換成 QUIC client** 修改```--plugin-opts "tls"```當中的```tls```改為```quic```就行。


## 其他客戶端

### ios shadowrocket
下面會有 "外掛"點開他，選擇"v2ray-plugin"，地址和埠留白，"TLS"打開，路徑寫"/"，伺服器填寫你申請的域名，就可以勒。

### android shadosocks 
基本上兩個要同時服用，少一個都不行。
安裝完v2ray plugin後，外掛程式就會多一個選項，選擇他。選完後，點選下面的設定，Transport mode 選擇"websocket-tls"，Hostname填寫自己申請的域名，點擊右上角的勾勾 可以啟動勒。

* shadowsocks : 
* v2ray plugin:
    * github 倉庫：https://github.com/shadowsocks/v2ray-plugin-android
    * google play 位置：https://play.google.com/store/apps/details?id=com.github.shadowsocks.plugin.v2ray
## 參考文獻
* [shadowsocks/v2ray-plugin](https://github.com/shadowsocks/v2ray-plugin)
* [shadowsocks/shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)
* [ss v2ray-plugin 簡中教學](https://gist.github.com/Shuanghua/c9c448f9bd12ebbfd720b34f4e1dd5c6)
* [acme.sh 更深入的應用](https://github.com/Neilpang/acme.sh/wiki/%E8%AF%B4%E6%98%8E)
* [v2ray-plugin 之魔改](https://medium.com/@langleyhouge/v2ray-plugin-%E4%B9%8B%E9%AD%94%E6%94%B9-834eb790293c)
* [今天偶然发现simple-obfs已经废弃了，取而代之的是v2ray-plugin，于是我决定尝鲜一下](https://blog.m3chd09.com/2019/02/01/v2ray-plugin-for-shadowsocks.html)