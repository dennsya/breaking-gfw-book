# shadowsocks-libev + kcptun 搭建（奶茶版）
> 将socks5 流量 转换成UDP传输

## Server
> 请在**root**执行
> **环境Ubuntu**

### 安装环境
安装shadowsocks-libev
```
apt-get install shadowsocks-libev -y
```

### 先启动 shadowsocks-libev
```
ss-server \
-s 0.0.0.0 \
-p <远端主机埠> \
-k <密码> \
-m xchacha20-ietf-poly1305 \
-t 600 \
-d 8.8.8.8
```

**建议的加密演算法**
* chacha20-ietf-poly1305、xchacha20-ietf-poly1305
* **加速** aes-128-gcm、aes-192-gcm、aes-256-gcm


### 安装kcptun 一键安装版本
```
wget --no-check-certificate https://github.com/kuoruan/shell-scripts/raw/master/kcptun/kcptun.sh;\
chmod +x ./kcptun.sh;\
./kcptun.sh
```
ps.基本一路Enter即可，安装完之后Kcp交由Supervisor管理

Supervisor 相关命令：
```
service supervisord {start|stop|restart|status}
```
Kcp 相关命令:
```
supervisorctl {start|stop|restart|status} kcptun
```

## 常见问题
1. 可能会出现Kuptem (spawn ERROR)的错误，原因在于sever-config.json档案的内容错误

```
可以到 /usr/local/kcptun/sever-config.json 进行修改
```
2. 流量设定
 Client的rcvwnd和Server端的sndwnd建议逐步由小到大调整避免流量浪费。
 
---

## Client
### Windows版本

#### KCP工具设定
https://github.com/xtaci/kcptun/releases
32位系统下载：kcptun-windows-386-20160922.tar.gz
64位系统下载：kcptun-windows-amd64-20160922.tar.gz

图形化Kcptun工具
https://github.com/dfdragon/kcptun_gclient/releases

根据Server端呈现的配置档案填入Kcptun工具当中
![](https://i.imgur.com/gXdfEzx.png)

1. 本地监听埠: 随意设定，之后要给SS客户端使用
2. KCP伺服器地址与埠: 设定SS的伺服器地址，埠则是使用KCP设定的埠
3. 其余设定根据自身配置档案做调整，完成之后点选启动即可

#### shadowsocks客户端设定
1. 伺服器地址: 设成127.0.0.1
2. 埠: 设定同KCP工具的本地监听埠
3. 加密及密码: 同SS原本的设定

### Android版本
https://github.com/shadowsocks/kcptun-android/releases
建议由github下载v0.1.0的版本，Google Play商店的版本为0.0.4

手机端参数设定类似于下
```
key=very fast;crypt=none;mode=fast;mtu=1350;sndwnd=512;rcvwnd=512;datashard=10;parityshard=3;dscp=0;nocomp
```

## 实际测试结果
手机端要使用的话Server端的sndwnd至少要大于512，在WIFI环境下能正常使用，但在手机网路上不太能够使用。

## 参考文献
* [shadowsocks/kcptun](https://github.com/shadowsocks/kcptun)
* [kcptun 一键安装版本](https://blog.kuoruan.com/110.html)
* [kcptun Android版本设定](https://blog.kuoruan.com/111.html)