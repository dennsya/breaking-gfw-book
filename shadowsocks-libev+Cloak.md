# Shadowsocks + Cloak
> 封包流包括 TLS 1.2 ， 但没有CA注册、域名

[Cloak](https://github.com/cbeuw/Cloak)是基于[GoQuiet](https://github.com/cbeuw/GoQuiet)，可以说是GoQuiet一个升级版，主要改动/增加了下面的功能：
* 通过多路复用，与GoQuiet相比，Cloak可显著减少网页载入时间（[详情](https://github.com/cbeuw/Cloak/wiki/Web-page-loading-benchmarks)）
* 多使用者支援，可以为单个使用者提供QoS控制，例如流量使用限制和频宽控制。此外还有一个WEB面板。

## server 端 环境
首先安装shadowsocks-libev版本：
```
apt - y install shadowsocks - libev libsodium - dev
```

接著安装Cloak的server端：
> * 寻找最新的的[版本](https://github.com/cbeuw/Cloak/releases/)
```
wget https://github.com/cbeuw/Cloak/releases/download/v2.1.2/ck-server-linux-amd64-2.1.2
mv ck-server-linux-amd64-2.1.2 /usr/bin/ck-server
chmod +x /usr/bin/ck-server
```

### Cloak 配置
生成密匙对（逗号前面的是公钥，后面的是私钥）：
```
ck-server -k
```
生成一个使用者的UID:
```
ck-server -u
```
新建Cloak配置档案：
```
mkdir -p /etc/ck-server/ && nano /etc/ck-server/ckserver.json
```

修改如下内容配置：
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
    "<你生成的UID>"
  ],
  "RedirAddr": "204.79.197.200",
  "PrivateKey": "<私钥>",
  "AdminUID": "<生成UID>",
  "DatabasePath": "/etc/ck-server/userinfo.db",
  "StreamTimeout": 300
}
```

> 1. Cloak不仅仅只支援shadowsocks，还支援openvpn等，这里我是用shadowsocks，所以在ProxyBook下面更改shadowsocks服务埠与之对应即可。
> 2. BindAddr是Cloak监听（服务）埠，预设是使用的80/443，所以用来部署Cloak的伺服器最好不要装Nginx/Caddy/Apache之类的软体，不然埠会冲突。
> 3. BypassUID是指一个没有限制的使用者（不受QoS限制，不受管理员限制）
> 4. AdminUID是指定这个使用者提升为管理员许可权，这边我因为就是一个人用，所以我将AdminUID和BypassUID设定为同一个ID。
> 5. PrivateKey是上一步ck-server -k生成的私钥。
> 现在编辑shadowsocks-libev的配置档案：

### 启动 Shadowsocks + Cloak Server 端
```
ss-server \
-s 0.0.0.0 \
-k <密码> \
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
  "PublicKey": "<server上的公钥>",
  "ServerName": "www.bing.com",
  "NumConn": 4,
  "BrowserSig": "chrome",
  "StreamTimeout": 300
}
```

> 1. ServerName这个其实也应该要和服务端上的RedirAddr对应，即你可以自己随便找一个域名，PING一下这个域名的IP地址，域名就填在ServerName，而PING出来的IP就填在RedirAddr。
> 2. BrowserSig是你想让GFW认为你在使用哪款浏览器访问网站，参数支援chrome/firefox。
> 3. 关于Transport其实是支援两种模式的，预设是direct，还有一种是cdn，对cdn支援感兴趣的可以去看官方的文件，这里不多叙述。

### client 跑起来
```
ss-local \
-s <IP地址> \
-k <密码> \
-p 443 \
-l 1080 \
-m chacha20-ietf-poly1305 \
--plugin /Users/macbookair/ck-client-darwin-amd64-2.1.2 \
--plugin-opts /Users/macbookair/Cloak/example_config/ckclient.json
```
> * `--plugin `在[这里](https://github.com/cbeuw/Cloak/releases/)寻找，放个绝对位置
> * `--plugin-opts` 这边是`ckclient.json`文件的路径，一定要指到，不然会失效

## 参考文献
* [cbeuw/Cloak](https://github.com/cbeuw/Cloak)
* [Android plugin:cbeuw/Cloak-android](https://github.com/cbeuw/Cloak-android/releases) :必须搭配 android版的shadowsocks
* [Shadowsocks-libev配置‘Cloak混淆外挂’](https://briteming.blogspot.com/2019/10/shadowsockscloak.html)