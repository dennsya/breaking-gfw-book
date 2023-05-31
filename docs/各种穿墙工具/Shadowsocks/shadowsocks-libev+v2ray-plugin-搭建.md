# shadowsocks-libev + v2ray-plugin 搭建
> tls1.2, tls1.3 需要CA, 域名

## 事前准备
### 1. 域名申请：[https://my.freenom.com/](https://my.freenom.com/)
* 只要有心就可以找到免费的域名

> * 注册完的样子
> ![](https://i.imgur.com/jKn8ocQ.png)
> * 点选Manage domain 将自己的想要的ＩＰ填写进去
> ps. 可以ping 测试一下



### 2. 设定 cloudflare：https://www.cloudflare.com/

* cloudflare DNS-> 增加一条 A 记录；
    * name=域名
    * value=vpsIP
    * ttl=automatic
    * status=onlyDns
* cloudflare Crypto -> 
    * SSL = Flexible
    * 总是开启 HTTPS
* **很重要**：点选overview分页，会提示你回到 "https://my.freenom.com/" 修改一些 域名

---

## Server
### 1. go 环境搭建：
```
apt-get update;\
apt-get upgrade -y;\
wget https://dl.google.com/go/go1.12.6.linux-amd64.tar.gz;\
sudo tar -xvf go1.12.6.linux-amd64.tar.gz;\
sudo mv go /usr/local;\
export GOROOT=/usr/local/go;\
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```

### 2. build v2ray-plug(适用Linux / Mac):
```
git clone https://github.com/shadowsocks/v2ray-plugin.git;\
cd v2ray-plugin;\
go build;\
cp v2ray-plugin /usr/bin/v2ray-plugin
```
> ps. 如果希望可以一次喷出linux,mac, windows版本，在v2ray-plugin底下执行：
> 
> ```
> bash build-release.sh
> ```


### 3. shadowsocks-libev 安装
```
apt-get install shadowsocks-libev
```

### 4. acme指令码自动申请证书申请

#### 4. 1 申请证书前
**准备：**

到cloudflare页面，点选头像(右上角)，点选我的个人资料，在个人资讯页面寻找 API KEYs - > 全球API KEY，点选"view"，拷贝key。

**储存：**
* API KEY
* CloundFlare的登陆邮箱。

#### 4.2 申请证书

填入 4.1 储存的讯息，在server 终端执行
```
export CF_Email="原本注册CloundFlare的mail"; \
export CF_Key="你的 API KEY"; \
curl https://get.acme.sh | sh; \
bash acme.sh/acme.sh --issue --dns dns_cf -d 你的域名
```

> ps. 这步骤常常遇到问题，请保重。
> **完成后可以在 ```/root/.acme.sh/你的域名/``` 看到一大堆生成的凭证钥匙，要都有喔！**
> * 你的域名.cer
> * 你的域名.conf
> * 你的域名.csr
> * 你的域名.csr.conf
> * 你的域名.ga.key
> * ca.cer
> * fullchain.cer

### 5. 启动ss-server
> 设定基本上与shadowsocks-libev没有差别
> 注意事项：
> * 因为要模拟https，所以防火墙必须开放443埠（tcp/udp）都开
> ps 基本上所有的参数设定都可以在这里找到(https://github.com/shadowsocks/shadowsocks-libev)
> 

```
ss-server \
-s 0.0.0.0 \
-k 密码 \
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
host=自己申请的域名;\
loglevel=none"
```

* ```-p 443```模拟https，所以必须是。
* ```-plugin /usr/bin/v2ray-plugin```位置可以是绝对位置，如果止填写```v2ray-plugin```预设位置为```/usr/bin/```
* **更换成 QUIC server** 修改```--plugin-opts "tls"```当中的```tls```改为```quic```就行。

**建议的加密演算法**
* chacha20-ietf-poly1305、xchacha20-ietf-poly1305
* **加速** aes-128-gcm、aes-192-gcm、aes-256-gcm

---

## Client
```
ss-local \
-s 远端主机IP\
-k 密码 \
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
host=自己申请的域名"
```

* **更换成 QUIC client** 修改```--plugin-opts "tls"```当中的```tls```改为```quic```就行。


## 其他客户端

### ios shadowrocket
下面会有 "外挂"点开他，选择"v2ray-plugin"，地址和埠留白，"TLS"开启，路径写"/"，伺服器填写你申请的域名，就可以勒。

### android shadosocks 
基本上两个要同时服用，少一个都不行。
安装完v2ray plugin后，外挂程式就会多一个选项，选择他。选完后，点选下面的设定，Transport mode 选择"websocket-tls"，Hostname填写自己申请的域名，点选右上角的勾勾 可以启动勒。

* shadowsocks : 
* v2ray plugin:
    * github 仓库：https://github.com/shadowsocks/v2ray-plugin-android
    * google play 位置：https://play.google.com/store/apps/details?id=com.github.shadowsocks.plugin.v2ray

## 参考文献
* [shadowsocks/v2ray-plugin](https://github.com/shadowsocks/v2ray-plugin)
* [shadowsocks/shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)
* [ss v2ray-plugin 简中教学](https://gist.github.com/Shuanghua/c9c448f9bd12ebbfd720b34f4e1dd5c6)
* [acme.sh 更深入的应用](https://github.com/Neilpang/acme.sh/wiki/%E8%AF%B4%E6%98%8E)
* [v2ray-plugin 之魔改](https://medium.com/@langleyhouge/v2ray-plugin-%E4%B9%8B%E9%AD%94%E6%94%B9-834eb790293c)
* [今天偶然发现simple-obfs已经废弃了，取而代之的是v2ray-plugin，于是我决定尝鲜一下](https://blog.m3chd09.com/2019/02/01/v2ray-plugin-for-shadowsocks.html)