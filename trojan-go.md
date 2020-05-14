# trojan-go

**下载trojan-go**
最新版本请参考:https://github.com/p4gefau1t/trojan-go/releases/
```
wget https://github.com/p4gefau1t/trojan-go/releases/download/v0.4.8/trojan-go-linux-amd64.zip
```
**安装解压缩软件**
```
apt-get install unzip
```
**解压缩 trojan-go**
```
unzip trojan-go-linux-amd64.zip
```

**索取凭证**
```
./trojan-go -autocert request
```
> 如果遇到 注册过请修改 nano domain_info.json 再来一遍


**设置 trojan-go 设置文档**
```
nano config.json
```
**config.json 示范内容**
> 密码改成你高兴的
```
{
    "run_type": "server",
    "local_addr": "0.0.0.0",
    "local_port": 443,
    "remote_addr": "127.0.0.1",
    "remote_port": 80,
    "password": [
        "password"
    ],
    "ssl": {
        "cert": "/root/server.crt",
        "key": "/root/server.key"
    }
}
```

**caddy for trojan-go 设置**

编辑
```
nano /etc/caddy/Caddyfile
```
填入
> 请修改 你的域名、IP
```
你的域名:80 {
    reverse_proxy IP:端口
}
```
> 修改完成后 `systemctl restart caddy`启动 caddy
> 检查caddy 是否启动成功 `systemctl status caddy`
> 如果成功就可以回到 trojan-go 启动最后一个步骤

**运行 trojan-go**
> 运行前要先搭建完毕返向代理 caddy or nginx 起个 80 port，不然无法启动
> 指令最后的 & 是为了背景运行
```
./trojan-go -config config.json &
```


参考文献
* [Great Firewall Report](https://gfw.report/)
* [技术原理](https://trojan-gfw.github.io/trojan/protocol)
* [trojan-go](https://github.com/p4gefau1t/trojan-go)
* [trojan](https://github.com/trojan-gfw/trojan)
* [trojan 改进](https://github.com/yuchting/trojan)
* [vpstoolbox](https://github.com/johnrosen1/vpstoolbox)
  ![](https://i.imgur.com/B4PSTGU.png)