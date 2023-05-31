# shadowsocks-libev + simple-obfs 搭建
> 将封包伪装为http

## Server
> 请在**root**执行
> **环境Ubuntu**

### 安装环境
安装shadowsocks-libev
```
apt-get install shadowsocks-libev -y
```
安装simple-obfs:
```
apt-get install --no-install-recommends build-essential autoconf libtool libssl-dev libpcre3-dev libev-dev asciidoc xmlto automake -y
git clone https://github.com/shadowsocks/simple-obfs.git \
cd simple-obfs \
git submodule update --init --recursive \
./autogen.sh \
./configure && make \
make install
```

### 启动 Server(http/tls)

```
ss-server \
-s 0.0.0.0 \
-p <远端主机端口> \
-k <密码> \
-m xchacha20-ietf-poly1305 \
-t 600 \
-d 8.8.8.8 \
--fast-open \
--plugin obfs-server \
--plugin-opts "obfs=http"
```
* ```obfs=http```把```http``` 改成 ```tls```模式就切换啦

**建议的加密算法**
* chacha20-ietf-poly1305、xchacha20-ietf-poly1305
* **加速** aes-128-gcm、aes-192-gcm、aes-256-gcm

---

## Client
> **环境mac os**

### 安装环境
安装shadowsocks-libev
```
brew upgrade shadowsocks-libev
```
安装simple-obfs:
```
brew install simple-obfs
```

### 启动 Client (http/tls)

```
ss-local \
-s <远端主机IP> \
-p <远端转机端口> \
-k <密码> \
-l <本地socks5端口> \
-m xchacha20-ietf-poly1305 \
-t 600 \
--fast-open \
--plugin obfs-local \
--plugin-opts "obfs=http;obfs-host=www.bing.com"
```
* ```host=www.bing.com```挑一个中国响叮当的域名
* ```obfs=http```把```http``` 改成 ```tls```模式就切换啦

## 参考文献
* [shadowsocks/simple-obfs](https://github.com/shadowsocks/simple-obfs)
* [shadowsocks/simple-obfsclient 指令](https://github.com/shadowsocks/simple-obfs/blob/master/doc/obfs-local.asciidoc)
* [shadowsocks/simple-obfsserver 指令](https://github.com/shadowsocks/simple-obfs/blob/master/doc/obfs-server.asciidoc)
* [MacOS使用shadowsocks-libev+Simple-OBFS教程](https://medium.com/@yanlong/macos%E4%BD%BF%E7%94%A8shadowsocks-libev-simple-obfs%E6%95%99%E7%A8%8B-c10eba9c0758)