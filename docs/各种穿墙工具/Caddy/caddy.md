# caddy
## 安裝方法一
```
echo "deb [trusted=yes] https://apt.fury.io/caddy/ /" \
    | sudo tee -a /etc/apt/sources.list.d/caddy-fury.list

sudo apt update
sudo apt install caddy
```

---

## 安裝方法二
下载caddy
> 最新版本参考:https://github.com/caddyserver/caddy/releases


```
wget https://github.com/caddyserver/caddy/releases/download/v2.0.0/caddy_2.0.0_linux_amd64.tar.gz
```
安装解压缩：
```
apt-get install tar
```

解压缩caddy： 
```
tar zxvf caddy_2.0.0_linux_amd64.tar.gz
```
把caddy 移入系统内部：
```
mv caddy /usr/bin/
```
测试caddy 指令：
```
caddy version
```
版本结果显示：
```
v2.0.0 h1:pQSaIJGFluFvu8KDGDODV8u4/QRED/OPyIR+MWYYse8
```


加进系统server控制的一部分

输入:
```
groupadd --system caddy
```
输入:
```
useradd --system \
	--gid caddy \
	--create-home \
	--home-dir /var/lib/caddy \
	--shell /usr/sbin/nologin \
	--comment "Caddy web server" \
	caddy
```
添加文档
```
nano /etc/systemd/system/caddy.service
```
粘贴以下内容存盘
```
# caddy.service
#
# For using Caddy with a config file.
#
# Make sure the ExecStart and ExecReload commands are correct
# for your installation.
#
# See https://caddyserver.com/docs/install for instructions.
#
# WARNING: This service does not use the --resume flag, so if you
# use the API to make changes, they will be overwritten by the
# Caddyfile next time the service is restarted. If you intend to
# use Caddy's API to configure it, add the --resume flag to the
# `caddy run` command or use the caddy-api.service file instead.

[Unit]
Description=Caddy
Documentation=https://caddyserver.com/docs/
After=network.target

[Service]
User=caddy
Group=caddy
ExecStart=/usr/bin/caddy run --environ --config /etc/caddy/Caddyfile
ExecReload=/usr/bin/caddy reload --config /etc/caddy/Caddyfile
TimeoutStopSec=5s
LimitNOFILE=1048576
LimitNPROC=512
PrivateTmp=true
ProtectSystem=full
AmbientCapabilities=CAP_NET_BIND_SERVICE

[Install]
WantedBy=multi-user.target
```

---

## 伺服器操作指令

完成存盘后可以使用以下指令控制服务器
```
软重启
sudo systemctl daemon-reload

开机自动运行
sudo systemctl enable caddy

启动服务器
sudo systemctl start caddy

服务器状态
systemctl status caddy
```

设置文件的位置：`/etc/caddy/Caddyfile`

**caddy for trojan-go 设置**

编辑
```
nano /etc/caddy/Caddyfile
```
填入
> 请修改 你的域名、IP
> 這邊是用來反向代理一個假的網站，倘若gfw來找你網站，會自動送過去這個IP:端口


```
你的域名:80 {
    reverse_proxy IP:端口
}
```
> 修改完成后 `systemctl restart caddy`启动 caddy
> 检查caddy 是否启动成功 `systemctl status caddy`
> 如果成功就可以回到 trojan-go 启动最后一个步骤


参考文献
* https://caddyserver.com/docs/v2-upgrade