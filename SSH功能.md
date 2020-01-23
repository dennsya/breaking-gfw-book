# SSH 功能

正向代理、反向代理、ssh-tunnel、ssh over socks5、交换钥匙
![](/image/ssh1.png)

* **一般连线**：
`ssh <user>@<ip> -p <port>`
* **正向代理（left-connection）**-远端port 投射到 本地port：
`ssh -LN <本地IP>:<本地Port>:<远端IP>:<远端Port> <远端User>@<远端IP> `
    * 持续尝试连线（要免密、依赖autossh）
`autossh -LN <本地IP>:<本地Port>:<远端IP>:<远端Port> <远端User>@<远端IP> `
* **反向代理（right-connection）**-本地port 投射到 远端port：
`ssh -RN <远端IP>:<远端Port>:<本地IP>:<本地Port> <远端User>@<远端IP>`
    * 持续尝试连线（要免密、依赖autossh）
`autossh -RN <远端IP>:<远端Port>:<本地IP>:<本地Port> <远端User>@<远端IP>`
ps. 光是这样是不够的，反向代理还必须修改，本地端`/etc/ssh/sshd_config`文件，加入`GatewayPorts yes`这一行。并且重新启动`sudo service ssh restart`

* **ssh-tunnel-本地socks5**：
`ssh -DN <本地Port> <远端User>@<远端IP>`
* **ssh over socks5**：
`ssh <远端User>@<远端IP> -o ProxyCommand="nc -X 5 -x 127.0.0.1:1080 %h %p"`

* **免密码登入/交换钥匙**：
* 如果本地没有钥匙，那就生成钥匙：`ssh-keygen -t rsa -C <自己的email>`
* 丢到远端主机：`ssh-copy-id <远端User>@<远端IP>`


## 开机自动执行
* 编辑`/etc/rc.local`文件
* 在`exit`之前加入指令
范例：
```
```


## 常见指令
```
-f 后台执行 ssh 指令
-C 允许压缩数据
-N 不执行远程指令
-R 将远程主机（服务器）的某个端口转发到本地端指定主机的指定端口
-L 将本地机（客户机）的某个端口转发到远端指定主机的指定端口
-p 指定远程主机的端口
```

## 参考文献
* [反向SSH实现内网穿透](https://cycoe.cc/2019/04/30/%E5%8F%8D%E5%90%91SSH%E5%AE%9E%E7%8E%B0%E5%86%85%E7%BD%91%E7%A9%BF%E9%80%8F/)
* [SSH隧道-本地和远程端口转发示例说明](https://blog.trackets.com/2014/05/17/ssh-tunnel-local-and-remote-port-forwarding-explained-with-examples.html)
* [反向建立 SSH Tunnel、免 VPN 连回公司](http://josephj.com/entry.php?id=312)