# shadowsocks-libev + kcptun 搭建
**[ss-server]<-->[kecptu-server]<-->[kecptu-client]<-->[ss-client]**

## Server
> 請在**root**執行
> **環境Ubuntu**

### 安裝環境
#### 安裝shadowsocks-libev
```
apt-get install shadowsocks-libev -y
```

#### go 環境搭建：
```
apt-get update;\
apt-get upgrade -y;\
wget https://dl.google.com/go/go1.12.6.linux-amd64.tar.gz;\
sudo tar -xvf go1.12.6.linux-amd64.tar.gz;\
sudo mv go /usr/local;\
export GOROOT=/usr/local/go;\
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```

#### 下載 kcptun+編譯 kcptun
```
go get -u github.com/xtaci/kcptun/... ;\
cd ~/go/src/github.com/xtaci/kcptun/ ;\
bash build-release.sh
```
#### 檢查 編譯完成的 kcptun
```
cd ~/go/src/github.com/xtaci/kcptun/build
```
![](https://i.imgur.com/XiehYUj.png)


### 啟動 shadowsocks-libev
```
ss-server \
-s 0.0.0.0 \
-p <遠端ss-server端口> \
-k <密碼> \
-m rc4-md5 \
-t 300 \
-d 8.8.8.8
```

### 啟動 kcptun_server
```
cd ~/go/src/github.com/xtaci/kcptun/build \
./server_linux_amd64 \
--target :<遠端ss-server端口> \
--localaddr :<遠端kcptun_server端口> \
--key xxx \
--crypt aes \
--mode fast3
```
#### 參數說明
* `--mode` : fast3, fast2, fast, normal, manual (default: "fast")
* `--crypt` : aes, aes-128, aes-192, salsa20, blowfish, twofish, cast5, 3des, tea, xtea, xor, sm4, none (default: "aes")
* `--key` : 你高興的密碼

## Client

### 啟動 kcptun_server
在遠端編譯好的文件想辦法弄到本地，而且要挑對檔案
```
./client_darwin_amd64  \
--localaddr :<本地kcptun_server端口> \
--remoteaddr <遠端主機IP>:<遠端kcptun_server端口> \
--key xxx \
--crypt aes \
--mode fast3
```

### 啟動 shadowsocks-libev
```
ss-local \
-s 0.0.0.0 \
-k xxx \
-p <本地kcptun_server端口> \
-l <本地socks5端口> \
-m rc4-md5 \
-t 300
```



## 參考文獻
* [skywind3000/kcp](https://github.com/skywind3000/kcp)
* [shadowsocks/kcptun](https://github.com/shadowsocks/kcptun)
* [Shadowsocks + KCPTun + UDP2RAW 超高速科学上网](https://www.lijingquan.net/index.php/2018/08/27/shadowsocks-kcptun-udp2raw-%E8%B6%85%E9%AB%98%E9%80%9F%E7%A7%91%E5%AD%A6%E4%B8%8A%E7%BD%91/)
* [一步一步教你用Kcptun给Shadowsocks加速！看YouTube1080P一点都不卡！](https://www.gblm.net/209.html)
