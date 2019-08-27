![](https://i.imgur.com/reHGdZ1.png =50x)![](https://i.imgur.com/cYvGvF0.png =190x)
# shadowsocks-libev + kcptun 搭建（奶茶版）


## Server
> 請在**root**執行
> **環境Ubuntu**

### 安裝環境
安裝shadowsocks-libev
```
apt-get install shadowsocks-libev -y
```

### 先啟動 shadowsocks-libev
```
ss-server \
-s 0.0.0.0 \
-p 遠端主機端口 \
-k 密碼 \
-m xchacha20-ietf-poly1305 \
-t 600 \
-d 8.8.8.8
```

**建議的加密算法**
* chacha20-ietf-poly1305、xchacha20-ietf-poly1305
* **加速** aes-128-gcm、aes-192-gcm、aes-256-gcm


### 安裝kcptun 一鍵安裝版本
```
wget --no-check-certificate https://github.com/kuoruan/shell-scripts/raw/master/kcptun/kcptun.sh;\
chmod +x ./kcptun.sh;\
./kcptun.sh
```
ps.基本一路Enter即可，安裝完之後Kcp交由Supervisor管理

Supervisor 相關命令：
```
service supervisord {start|stop|restart|status}
```
Kcp 相關命令:
```
supervisorctl {start|stop|restart|status} kcptun
```

## 常見問題
1. 可能會出現Kuptem (spawn ERROR)的錯誤，原因在於sever-config.json文件的內容錯誤

```
可以到 /usr/local/kcptun/sever-config.json 進行修改
```
2. 流量設置
 Client的rcvwnd和Server端的sndwnd建議逐步由小到大調整避免流量浪費。
 
---

## Client
### Windows版本

#### KCP工具設定
https://github.com/xtaci/kcptun/releases
32位系统下载：kcptun-windows-386-20160922.tar.gz
64位系统下载：kcptun-windows-amd64-20160922.tar.gz

圖形化Kcptun工具
https://github.com/dfdragon/kcptun_gclient/releases

根據Server端呈現的配置文件填入Kcptun工具當中
![](https://i.imgur.com/gXdfEzx.png)

1. 本地監聽端口: 隨意設定，之後要給SS客戶端使用
2. KCP服務器地址與端口: 設定SS的伺服器地址，端口則是使用KCP設定的端口
3. 其餘設置根據自身配置文件做調整，完成之後點選啟動即可

#### shadowsocks客戶端設定
1. 伺服器地址: 設成127.0.0.1
2. 端口: 設定同KCP工具的本地監聽端口
3. 加密及密碼: 同SS原本的設定

### Android版本
https://github.com/shadowsocks/kcptun-android/releases
建議由github下載v0.1.0的版本，Google Play商店的版本為0.0.4

手機端參數設置類似於下
```
key=very fast;crypt=none;mode=fast;mtu=1350;sndwnd=512;rcvwnd=512;datashard=10;parityshard=3;dscp=0;nocomp
```

## 實際測試結果
手機端要使用的話Server端的sndwnd至少要大於512，在WIFI環境下能正常使用，但在手機網路上不太能夠使用。

## 參考文獻
* [shadowsocks/kcptun](https://github.com/shadowsocks/kcptun)
* [kcptun 一鍵安裝版本](https://blog.kuoruan.com/110.html)
* [kcptun Android版本設定](https://blog.kuoruan.com/111.html)