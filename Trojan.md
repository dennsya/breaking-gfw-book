# Trojan

## 準備材料
* 申請域名：[https://www.freenom.com](https://www.freenom.com)
* 註冊cloudflare：[cloudflare](cloudflare)
> 把 freenode 的DNS 控制權 交給 cloudflare
> 準備材料其實跟 **SS + v2ray-plugin 搭建** 大同小異，可以去參考。

## 伺服器搭建環境
### 創建帳戶
* 創建群組certusers
* 創建 trojan,acme 屬於 certusers
```
sudo groupadd certusers
sudo useradd -r -M -G certusers trojan
sudo useradd -r -m -G certusers acme
```

### 環境
一鍵安裝所有套件
```
sudo apt update ; sudo apt upgrade -y; sudo apt install -y socat cron curl libcap2-bin xz-utils nginx
```

> 安裝環境相關套件
> ```
> sudo apt update
> sudo apt upgrade -y
> 
> 安裝跟acme.sh 相關的套件(憑證)
> sudo apt install -y socat cron curl
> 
> 安裝跟 trojan 相關的套件
> sudo apt install -y libcap2-bin xz-utils nano
> 
> 安裝 nginx 伺服器相關套件
> sudo apt install -y nginx
> ```

啟動 **cron** 相關套件
```
sudo systemctl start cron
sudo systemctl enable cron
```

### 憑證
創建憑證文件夾
```
sudo mkdir /usr/ local/etc/certfiles 
sudo chown -R acme:acme /usr/local/etc/certfiles
```

設定憑證
```
進入 acme 用戶
sudo su -l -s /bin/bash acme

下載憑證工具
curl https://get.acme.sh | sh 

退出 acme 用戶
exit

再次進入 acme 用戶
sudo su -l -s /bin/bash acme
```

設定 cloudfare 的環境變數，用於生成憑證
```
export CF_Key= "<Your Global API Key>" 
export CF_Email= "<Your cloudflare account Email>"
```

申請憑證 **<tdom.ml>**改成自己的域名
```
acme.sh --issue --dns dns_cf -d <tdom.ml>
```

安裝憑證 **<tdom.ml>**改成自己的域名
```
acme.sh --install-cert -d <tdom.ml> --key-file /usr/ local/etc/certfiles/private.key --fullchain-file /usr/ local/etc/certfiles/certificate.crt
```
自動更新證書
```
acme.sh --upgrade --auto-upgrade
```

修改權限
```
chown -R acme:certusers /usr/ local/etc/certfiles 
chmod -R 750 /usr/ local/etc/certfiles 
exit
```
### 安裝 Trojan
```
sudo bash -c " $(curl -fsSL https://raw.githubusercontent.com/trojan-gfw/trojan-quickstart/master/trojan-quickstart.sh)"
sudo chown -R trojan:trojan /usr/ local/etc/trojan 
sudo cp /usr/ local/etc/trojan/config.json /usr/ local/etc/trojan/config.json.bak 
```
修改配置文件
```
sudo nano /usr/ local/ etc/trojan/config.json
```
* password 修改成自己的, 多的刪掉 注意要刪掉逗點
* cert和key分別改為/usr/local/etc/certfiles/certificate.crt和/usr/local/etc/certfiles/private.key

### 啟動Trojan
編輯 Trojan
```
sudo nano /etc/systemd/system/trojan.service
```
> 在 `[server]` 添加`User=trojan`
重新讀取
```
sudo systemctl daemon-reload
```
賦予Trojan監聽443端口能力
```
sudo setcap CAP_NET_BIND_SERVICE=+eip /usr/ local/bin/trojan
```

使用systemd啟動Trojan

```
重起 Trojan
sudo systemctl restart trojan 

檢查 Trojan
sudo systemctl status trojan
```

### 設定 nginx
關閉nginx預設主機

```
sudo rm /etc/nginx/sites-enabled/default
```

寫入新的設定 **<tdom.ml>**改成自己的域名

```
sudo nano /etc/nginx/sites-available/<tdom.ml>
```

修改文件內容

```
server { 
    listen 127.0.0.1:80 default_server; 
    server_name <tdom.ml>; 
    location / { 
        proxy_pass https://www.ietf.org; 
    } 
} 
server { 
    listen 127.0.0.1:80; 
    server_name <10.10.10.10>; return 301 https://<tdom.ml> $request_uri; } server {     listen 0.0.0.0:80;     listen [::]:80;     server_name _; return 301 https:// $host $request_uri; }
```

* server_name <tdom.ml> ，修改為自己的域名
* `proxy_pass https://www.ietf.org` 修改成不敏感的網站
* `server_name <10.10.10.10>`修改成自己的IP
* https://<tdom.ml>，修改為自己的域名

使能配置文件注意域名<tdom.ml>改為你自己的域名

```
sudo ln -s /etc/nginx/sites-available/<tdom.ml> /etc/nginx/sites-enabled/
```

### 啟動Nginx

```
sudo systemctl restart nginx 
sudo systemctl status nginx
```

配置Trojan和Nginx開機自啟
```
sudo systemctl enable trojan 
sudo systemctl enable nginx
```

## 參考文獻
* [自建梯子教程 --Trojan版本](https://www.cnblogs.com/z45281625/p/11738850.html)