# shadowsocks-libev 搭建
> 最基础的socks包装

## Server
#### ubuntu中安装shadowsocks-libev
```
apt-get install shadowsocks-libev
```

### server 启动指令
在Ubuntu 终端中输入：
```
ss-server \
-s 0.0.0.0 \
-p <远端主机端口> \
-k <密码> \
-m xchacha20-ietf-poly1305 \
-t 600 \
-d 8.8.8.8 \
--fast-open 
```
**建议的加密算法**
* chacha20-ietf-poly1305、xchacha20-ietf-poly1305
* **加速** aes-128-gcm、aes-192-gcm、aes-256-gcm

---

## Client
### mac os 中安装shadowsocks-libev
```
brew install shadowsocks-libev
```
### 一般client启动指令
在Mac os 终端输入：
```
ss-local \
-s <远端主机IP> \
-p <远端主机端口> \
-k <密码> \
-l <本地socks5 端口> \
-m xchacha20-ietf-poly1305 \
-t 600
--fast-open
```

## 参考文献
* [shadowsocks/shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)