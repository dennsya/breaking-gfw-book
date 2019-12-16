# Shadowsocks + Cloak

[Cloak](https://github.com/cbeuw/Cloak)是基於[GoQuiet](https://github.com/cbeuw/GoQuiet)，可以說是GoQuiet一個升級版，主要改動/增加了下面的功能：
* 通過多路復用，與GoQuiet相比，Cloak可顯著減少網頁載入時間（[詳情](https://github.com/cbeuw/Cloak/wiki/Web-page-loading-benchmarks)）
* 多使用者支援，可以為單個使用者提供QoS控制，例如流量使用限制和頻寬控制。此外還有一個WEB面板。

## server 端 環境
首先安裝shadowsocks-libev版本：
```
apt - y install shadowsocks - libev libsodium - dev
```

接著安裝Cloak的server端：
> * 尋找最新的的[版本](https://github.com/cbeuw/Cloak/releases/)
```
wget https://github.com/cbeuw/Cloak/releases/download/v2.1.2/ck-server-linux-amd64-2.1.2
mv ck-server-linux-amd64-2.1.2 /usr/bin/ck-server
chmod +x /usr/bin/ck-server
```

### Cloak 配置
生成密匙對（逗號前面的是公鑰，後面的是私鑰）：
```
ck-server -k
```
生成一個使用者的UID:
```
ck-server -u
```
新建Cloak配置檔案：
```
mkdir -p /etc/ck-server/ && nano /etc/ck-server/ckserver.json
```

修改如下內容配置：
```
{
  "ProxyBook": {
    "shadowsocks": [
      "tcp",
      "127.0.0.1:51234"
    ],
    "openvpn": [
      "udp",
      "127.0.0.1:8389"
    ],
    "tor": [
      "tcp",
      "127.0.0.1:9001"
    ]
  },
  "BindAddr": [
    ":443",
    ":80"
  ],
  "BypassUID": [
    "你生成的UID"
  ],
  "RedirAddr": "204.79.197.200",
  "PrivateKey": "<私鑰>",
  "AdminUID": "<生成UID>",
  "DatabasePath": "/etc/ck-server/userinfo.db",
  "StreamTimeout": 300
}
```

> 1.Cloak不僅僅只支援shadowsocks，還支援openvpn等，這裡我是用shadowsocks，所以在ProxyBook下面更改shadowsocks服務埠與之對應即可。
> 2.BindAddr是Cloak監聽（服務）埠，預設是使用的80/443，所以用來部署Cloak的伺服器最好不要裝Nginx/Caddy/Apache之類的軟體，不然埠會衝突。
> 3.BypassUID是指一個沒有限制的使用者（不受QoS限制，不受管理員限制）
> 4.AdminUID是指定這個使用者提升為管理員許可權，這邊我因為就是一個人用，所以我將AdminUID和BypassUID設定為同一個ID。
> 5.PrivateKey是上一步ck-server -k生成的私鑰。
> 現在編輯shadowsocks-libev的配置檔案：

### 啟動 Shadowsocks + Cloak Server 端
```
ss-server \
-s 0.0.0.0 \
-k <密碼> \
-p 51234 \
-d 8.8.8.8 \
-t 60 \
-m chacha20-ietf-poly1305 \
--plugin ck-server \
--plugin-opts /etc/ck-server/ckserver.json
```

## Mac Client 端
配置文件ckclient.json：
```
{
  "Transport": "direct",
  "ProxyMethod": "shadowsocks",
  "EncryptionMethod": "plain",
  "UID": "<server上的UID>",
  "PublicKey": "<server上的公鑰>",
  "ServerName": "www.bing.com",
  "NumConn": 4,
  "BrowserSig": "chrome",
  "StreamTimeout": 300
}
```

> 1.ServerName這個其實也應該要和服務端上的RedirAddr對應，即你可以自己隨便找一個域名，PING一下這個域名的IP地址，域名就填在ServerName，而PING出來的IP就填在RedirAddr。
> 2.BrowserSig是你想讓GFW認為你在使用哪款瀏覽器訪問網站，參數支援chrome/firefox。
> 3.關於Transport其實是支援兩種模式的，預設是direct，還有一種是cdn，對cdn支援感興趣的可以去看官方的文件，這裡不多敘述。

### client 跑起來
```
ss-local \
-s <IP地址> \
-k <密碼> \
-p 443 \
-l 1080 \
-m chacha20-ietf-poly1305 \
--plugin /Users/macbookair/ck-client-darwin-amd64-2.1.2 \
--plugin-opts /Users/macbookair/Cloak/example_config/ckclient.json
```
> * `--plugin `在[這裡](https://github.com/cbeuw/Cloak/releases/)尋找，放個絕對位置
> * `--plugin-opts` 這邊是`ckclient.json`文件的路徑，一定要指到，不然會失效

## 參考文獻
* [cbeuw/Cloak]https://github.com/cbeuw/Cloak
* [Android plugin:cbeuw/Cloak-android](https://github.com/cbeuw/Cloak-android/releases) :必須搭配 android版的shadowsocks
* [Shadowsocks-libev配置‘Cloak混淆外掛’](https://briteming.blogspot.com/2019/10/shadowsockscloak.html)